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

