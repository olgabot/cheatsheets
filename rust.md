# Rust

## Print the values of a hashmap for debugging purposes

```rust

pub fn print_value_count(hashes_with_abund: Vec<(u64, u64)>) -> HashMap<u64, u64> {
    // Coun the number of times the abundances appear in a hashes with abundance output
    let mut value_counts: HashMap<u64, u64>  = HashMap::new();

    for (_key, value) in hashes_with_abund.iter() {
        // Count number of times we see each value
        *value_counts.entry(*value).or_insert(0) += 1;
    }

    for (value, count) in value_counts.iter() {
        eprintln!("value: {}, count: {}", value, count);
    }

    return value_counts;
}

pub fn print_freq_count(frequencies: HashMap<&u64, f64>, freq_type: &str, query_name: &str, n_hashes: usize) -> HashMap<String, u64> {
    // Coun the number of times the abundances appear in a hashes with abundance output
    let mut freq_counts: HashMap<String, u64>  = HashMap::new();

    for (_key, value) in frequencies.iter() {
        // Count number of times we see each value
        *freq_counts.entry(value.to_string()).or_insert(0) += 1;
    }

    for (freq, count) in freq_counts.iter() {
        eprintln!("{}\t{}\tn_hashes: {}\tfreq: {}, count: {}", freq_type, query_name, n_hashes, freq, count);
    }

    return freq_counts;
}

```
