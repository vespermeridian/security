# Wordlist Generation Playbook

## Creating wordlists based on profiling

### *CUPP - Common user password profiler*
```python3 cupp.py -i```

## Creating wordlists based on target

### *CeWL - Creating lists from sites*

```cewl -d <depth to spider> -m <minimum word length> -w <output wordlist> <url of website>```

### *PrinceProcessor - Word chaining*

```
./pp64.bin -o wordlist.txt < words
```


## Tools / Installation instructions

### CUPP
Repo: [CUPP](https://github.com/Mebus/cupp)

### CeWL
Repo: [CeWL](https://github.com/digininja/CeWL)

### PrinceProcessor
```
wget https://github.com/hashcat/princeprocessor/releases/download/v0.22/princeprocessor-0.22.7z
7z x princeprocessor-0.22.7z
cd princeprocessor-0.22
./pp64.bin -h
```