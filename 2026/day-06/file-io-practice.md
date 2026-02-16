* Create a file
* Write text
* Append new lines
* Read file contents

---

# 🧪 Step 1 – Create a File

### Method 1 (Simple empty file)

```bash
touch demo.txt
```

Verify:

```bash
ls -l demo.txt
```

👉 Creates an empty file named `demo.txt`

---

# 🧪 Step 2 – Write Text to a File (Overwrite Mode)

```bash
echo "Hello, this is my first file." > demo.txt
```

👉 `>` means **overwrite**
If file already has content, it will be replaced.

---

# 🧪 Step 3 – Append New Lines

```bash
echo "This line is appended." >> demo.txt
echo "Practicing Linux file operations." >> demo.txt
```

👉 `>>` means **append**
Adds content to end of file.

---

# 🧪 Step 4 – Read the File

### Option 1 – cat

```bash
cat demo.txt
```

### Option 2 – less (for bigger files)

```bash
less demo.txt
```

Press `q` to exit.

### Option 3 – head / tail

```bash
head demo.txt
tail demo.txt
```

---

# 🧪 Step 5 – Use nano (Manual Edit)

```bash
nano demo.txt
```

* Edit content
* Press `CTRL + X`
* Press `Y`
* Press `Enter`

---

# 🔁 Repeatable Practice Script

You can copy-paste this whole block daily:

```bash
touch practice.txt
echo "DevOps Practice Day 1" > practice.txt
echo "Learning file operations." >> practice.txt
echo "Appending more data." >> practice.txt
cat practice.txt
```

---

# 🧠 What You Learned

| Command   | Purpose           |
| --------- | ----------------- |
| touch     | Create empty file |
| echo >    | Write (overwrite) |
| echo >>   | Append            |
| cat       | Read file         |
| head/tail | Read partial      |
| nano      | Edit manually     |

---

# 🔥 Mini Challenge

Try this:

1. Create file named `notes.txt`
2. Add your name
3. Add today's date
4. Append "Practicing Linux fundamentals"
5. Display last 2 lines only

Solution hint:

```bash
tail -n 2 notes.txt
```
