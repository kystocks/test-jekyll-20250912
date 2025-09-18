# DNS Lab Answers - SI 676 Lab 3
**Student:** Kyle Stocksdale  
**Date:** September 17, 2025

## DNS Question 1: Explain the `dig umich.edu` response

When I ran `dig umich.edu`, the response shows several important sections:

**ANSWER SECTION:**
- `umich.edu` resolves to two IPv4 addresses:
  - `172.66.0.37`
  - `162.159.140.37`
- Both have a TTL (Time To Live) of 1800 seconds (30 minutes)
- These are A records (IPv4 address records)

**AUTHORITY SECTION:**
- Shows the authoritative name servers for the umich.edu domain:
  - `dns1.itd.umich.edu`
  - `dns3.umich.org`
  - `dns4.umich.org`
  - `dns2.itd.umich.edu`
- These are NS (Name Server) records with TTL of 172800 seconds (48 hours)

**ADDITIONAL SECTION:**
- Provides the IP addresses for the name servers listed in the authority section
- Includes both IPv4 (A records) and IPv6 (AAAA records) for the name servers

**Explanation:**
This response demonstrates DNS redundancy and load balancing. The multiple IP addresses for umich.edu allow traffic distribution and failover capability. The multiple authoritative name servers ensure DNS resolution reliability. The query took 26 milliseconds, showing efficient DNS infrastructure.

## DNS Question 2: Piping output to file

**Command used:** `dig umich.edu > umich-dns-record.txt`

The output was successfully saved to the text file `umich-dns-record.txt` in my working directory.

## DNS Question 3: Return only "answer" section

**Command:** `dig +noall +answer umich.edu`

**Result:**
```
umich.edu.		1800	IN	A	162.159.140.37
umich.edu.		1800	IN	A	172.66.0.37
```

This command filters the dig output to show only the answer section, providing a clean view of just the domain-to-IP mappings without the additional DNS infrastructure information.

## Bonus Question: CSV-like output with awk

**Command:** `dig +noall +answer si.umich.edu umich.edu wisc.edu education.wisc.edu | awk '{print $1","$5}'`

**Result:**
```
si.umich.edu.,162.159.140.37
si.umich.edu.,172.66.0.37
umich.edu.,172.66.0.37
umich.edu.,162.159.140.37
wisc.edu.,144.92.9.70
education.wisc.edu.,75.2.33.159
education.wisc.edu.,99.83.210.234
```

**Explanation:**
This command combines multiple DNS queries and uses `awk` to extract just the domain name (field 1) and IP address (field 5), formatting them as comma-separated values. This demonstrates:
- Multiple domain querying in a single dig command
- Text processing with awk to create structured output
- How different domains may have different numbers of IP addresses for load balancing

## Technical Observations

1. **Load Balancing:** Most domains queried have multiple IP addresses for redundancy
2. **DNS Infrastructure:** University domains use sophisticated DNS setups with multiple authoritative servers
3. **TTL Values:** Different record types have different cache lifespans (1800s for A records, 172800s for NS records)
4. **Geographic Distribution:** The various IP addresses likely represent geographically distributed servers