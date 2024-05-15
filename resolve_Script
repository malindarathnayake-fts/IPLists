#!/bin/bash

# URL of the file containing DNS entries
url="https://raw.githubusercontent.com/malindarathnayake-fts/IPLists/main/Shodan_IO_IPRanges"

# Output file name
output_file="resolved_ips.txt"

# Associative array to store unique IPs
declare -A unique_ips

# Fetch the file content from the URL
dns_entries=$(curl -s "$url")

# Split the content into lines
readarray -t dns_entries <<< "$dns_entries"

# Iterate over each DNS entry and resolve IP addresses
for entry in "${dns_entries[@]}"; do
    ips=$(getent ahostsv4 "$entry" | awk '{print $1}')
    if [ -n "$ips" ]; then
        # Split IPs into an array
        readarray -t ip_arr <<< "$ips"
        # Add each unique IP to the associative array
        for ip in "${ip_arr[@]}"; do
            unique_ips["$ip"]=1
        done
    else
        echo "Unable to resolve $entry" >&2
    fi
done

# Write unique IPs to the output file
for ip in "${!unique_ips[@]}"; do
    echo "$ip" >> "$output_file"
done
