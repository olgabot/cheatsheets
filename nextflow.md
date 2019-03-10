# Nextflow

## Using cached data

Specify `-resume`, e.g.:

```
nextflow run main.rf -resume
```

## AWS Setup at Biohub

Nextflow configuration file `nextflow.config`

```groovy
process.executor = 'awsbatch'
process.queue = 'nextflow'
process.container = 'ubuntu'

executor.awscli = '/home/ec2-user/miniconda/bin/aws'

aws {
	region = 'us-west-2'

    client {
        maxConnections = 20
        connectionTimeout = 10000
        uploadStorageClass = 'INTELLIGENT_TIERING'
        storageEncryption = 'AES256'
    }
}

executor {
         name = 'local'
         // Maximum number of cpus
         cpus = 4
}

cloud {
      // Amazon ECS-optimized Nextflow image with AWS cli
    imageId = 'ami-0c323ba3e98b979f9'
    instanceType = 'm4.xlarge'
    instanceStorageMount = "/mnt/data"
    subnetId = 'subnet-05222a43'
    autoscale {
              enabled = true
              maxInstances = 20
              terminateWhenIdle = true
              }
}

```

## What are all the output files?


### `command.sh`

The actual command you were running

### `command.run`

#### Nextflow-internal function definitions

A bunch of internal nextflow-defined bash functions:

- `nxf_env()`
- `nxf_kill()`
- `nxf_mktmp()`
- `on_exit()`
- `on_term()`
- `nxf_s3_upload()`
- `nxf_s3_download()`
- `nxf_parallel()`

#### Downloading data

downloading the data, e.g.:


```bash
echo start | /home/ec2-user/miniconda/bin/aws --region us-west-2 s3 cp --only-show-errors - s3://olgabot-maca/nextflow-bucket-dir-test/8e/15ae3133d7f8f163f0b6726edd375a/.command.begin
[[ $NXF_SCRATCH ]] && echo "nxf-scratch-dir $HOSTNAME:$NXF_SCRATCH" && cd $NXF_SCRATCH
# stage input files
downloads=()
rm -f L1-MAA000752-3_10_M-1-1_R1_001.fastq.gz
rm -f L1-MAA000752-3_10_M-1-1_R2_001.fastq.gz
rm -f .command.sh
rm -f .command.stub
downloads+=("nxf_s3_download s3://czb-maca/Plate_seq/3_month/170910_A00111_0054_AH2HGWDMXX__170910_A00111_0053_BH2HGKDMXX/fastqs/L1-MAA000752-3_10_M-1-1_R1_001.fastq.gz L1-MAA000752-3_10_M-1-1_R1_001.fastq.gz")
downloads+=("nxf_s3_download s3://czb-maca/Plate_seq/3_month/170910_A00111_0054_AH2HGWDMXX__170910_A00111_0053_BH2HGKDMXX/fastqs/L1-MAA000752-3_10_M-1-1_R2_001.fastq.gz L1-MAA000752-3_10_M-1-1_R2_001.fastq.gz")
downloads+=("nxf_s3_download s3://olgabot-maca/nextflow-bucket-dir-test/8e/15ae3133d7f8f163f0b6726edd375a/.command.sh .command.sh")
downloads+=("nxf_s3_download s3://olgabot-maca/nextflow-bucket-dir-test/8e/15ae3133d7f8f163f0b6726edd375a/.command.stub .command.stub")
nxf_parallel "${downloads[@]}"
```

#### Running `command.stub` 

Runs `command.stub` and saves the standard output/standard error to `.command.out` and `.command.err`


```bash
ctmp=$(set +u; nxf_mktemp /dev/shm 2>/dev/null || nxf_mktemp $TMPDIR)
cout=$ctmp/.command.out; mkfifo $cout
cerr=$ctmp/.command.err; mkfifo $cerr
tee .command.out < $cout &
tee1=$!
tee .command.err < $cerr >&2 &
tee2=$!
(
/bin/bash .command.stub
) >$cout 2>$cerr &
pid=$!
```
#### Wait for the command to finish and upload results

Wait for the process ID from above, and upload `.command.out`, `.command.err` files to bucket, and upload the final output

```
wait $pid || ret=$?
wait $tee1 $tee2
nxf_s3_upload .command.out s3://olgabot-maca/nextflow-bucket-dir-test/8e/15ae3133d7f8f163f0b6726edd375a || true
nxf_s3_upload .command.err s3://olgabot-maca/nextflow-bucket-dir-test/8e/15ae3133d7f8f163f0b6726edd375a || true
# copies output files to target
if [[ ${ret:=0} == 0 ]]; then
  uploads=()
  uploads+=("nxf_s3_upload 'L1-MAA000752-3_10_M-1-1.sig' s3://olgabot-maca/nextflow-bucket-dir-test/8e/15ae3133d7f8f163f0b6726edd375a")
  nxf_parallel "${uploads[@]}"
fi
nxf_s3_upload .command.trace s3://olgabot-maca/nextflow-bucket-dir-test/8e/15ae3133d7f8f163f0b6726edd375a || true
```


### `command.stub` - Run the command and monitor memory/cpu/etc

#### Internal Nextflow bash functions

- `nxf_pcpu()`
- `nxf_tree()`
- `nxf_pstat()`
- `nxf_sleep()`
- `nxf_date()`
- `nxf_trace()`


#### Run the command and save the output


```
trap 'exit ${ret:=$?}' EXIT
touch .command.trace
start_millis=$(nxf_date)
(
/bin/bash -ue .command.sh
) &
pid=$!
nxf_trace "$pid" .command.trace &
mon=$!
wait $pid || ret=$?
end_millis=$(nxf_date)
wait $mon
echo $((end_millis-start_millis)) >> .command.trace
```


