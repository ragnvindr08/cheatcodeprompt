# Windows CMD Cheat Sheet

---

## Process Management

```cmd
tasklist                        # list all processes
tasklist | findstr node         # find specific process
taskkill /PID 1234 /F           # kill by PID
taskkill /IM node.exe /F        # kill by name
netstat -ano                    # list all ports
netstat -ano | findstr :3000    # find process on port
```

---

## Edit File Lines

**Overwrite & Append**
```cmd
echo content > file.txt         # overwrite entire file
echo content >> file.txt        # append a line
```

**Replace a line**
```cmd
(for /f "tokens=*" %i in (file.txt) do (
  if "%i"=="old line" (echo new line) else (echo %i)
)) > temp.txt && move /y temp.txt file.txt
```

**Delete a line**
```cmd
(for /f "tokens=*" %i in (file.txt) do (
  if not "%i"=="line to delete" echo %i
)) > temp.txt && move /y temp.txt file.txt
```

> The `for /f` + temp file trick is the standard pure-CMD way to edit lines — CMD has no built-in in-place editor.

---

## Find Bugs & Errors

```cmd
findstr /i /n "error" file.log            # search with line numbers
findstr /i /s "error" *.log              # search recursively in subfolders
findstr /i "error exception fail" app.log # search multiple keywords
findstr /i /v "info" app.log             # exclude matching lines
findstr /r "^ERROR" app.log              # regex: lines starting with ERROR
```

**`findstr` Flags**

| Flag | Meaning |
|------|---------|
| `/i` | Case-insensitive |
| `/n` | Show line numbers |
| `/s` | Search subfolders |
| `/v` | Exclude matching lines |
| `/c:"text"` | Exact phrase match |
| `/r` | Use regex |

---

## Manual Debug Flow

```cmd
node app.js > output.log 2>&1    # 1. Run & capture errors
findstr /i /n "error" output.log  # 2. Find error lines
more output.log                   # 3. Read full log
notepad app.js                    # 4. Edit the fix
node app.js                       # 5. Re-run to verify
```

---

## Redirect Cheat Sheet

| CMD | Meaning |
|-----|---------|
| `>` | Write output to file |
| `>>` | Append output to file |
| `2>&1` | Capture errors too |
| `\|` | Pipe output to next command |
| `2> err.log` | Errors only to file |
