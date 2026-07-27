# Assignment 5 — Bash Script Automation Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will practice Bash scripting by building a series of small automation scripts covering environment setup, variables, arrays, loops, file conditionals, if-else logic, and functions. These scripts form the foundation of real-world Linux automation used in DevOps, cloud, and production support environments.

---

# Task 1 — Bash Environment & Workspace Setup

## Goal

Verify that Bash is available on your system and create a clean workspace for this assignment.

### Evidence

#### Screenshot 1 — Output of `echo $SHELL` and `bash --version`

![task 1](screenshots/Screenshot%20with%20echo%20shell.png)

--- 

#### Screenshot 2 — Output of `pwd` and `ls -lah` showing the scripts directory

![task 1](screenshots/Screenshot%20with%20pwd%20ls_lh.png)

--- 

### Notes

Answer the following in your own words:

**1. What is Bash?**

 Its a shell used on Linux systems. Its the short form for Bourne Again Shell and is a command-line shell also a scripting language. Bash reads commands entered by a user or stored in a script and requests the operating system to perform the required operations. 

---


**2. What is the difference between shell and Bash?**

"shell" is a broad category name for any command-line interpreter program, while "Bash" is one specific, highly popular implementation of a shell

 A shell is a software program that acts as an interface between a user and the operating system kernel. It takes commands typed by the user or read from a script file, interprets them, and passes them to the computer's operating system to execute

 Bash stands for Bourne-Again SHell. It is a specific type of shell.  It is the de facto default shell on almost all Linux distributions.

---  

**3. Why is it important to confirm the Bash version before writing scripts?**

It is important to confirm the bash version cos different versions introduce breaking syntax changes, new features, and array handling differences. Writing a script with newer features without confirming the environment will cause syntax errors on older systems . It ensures that the syntax and features used in our scripts are supported by the existing system. 

--- 

# Task 2 — Your First Bash Script

## Goal

Create your first Bash script, make it executable, and run it from the terminal.

### Evidence

#### Screenshot 1 — Content of `first-script.sh`

![task 2](screenshots/Screenshot%20with%20content%20of%20first%20script%20.png)

--- 

#### Screenshot 2 — Output of `./first-script.sh`

![task 2](screenshots/Screenshot%20with%20output%20of%20first%20script%20.png)

--- 

#### Screenshot 3 — Output of `ls -l first-script.sh` showing executable permission

![task 2](screenshots/Screenshot%20with%20output%20output%20putput.png)

--- 

### Notes

Answer the following in your own words:

**1. What is the purpose of `#!/bin/bash`?**

It talks to the OS(Ubuntu) to use the Bash interpreter to run the commands inside the script. 

---  


**2. Why do we use `chmod +x` before running a script?**

It adds the command "execute permission", which enables us to run the script directly for example above we jut invoke/use ./first-script.sh. 

--- 


**3. What is the difference between running a script using `./script.sh` and `bash script.sh`?**

./script.sh executes the script as a standalone program using the interpreter defined inside the script,

 while bash script.sh forces the script to run explicitly through the Bash interpreter, bypassing the script's internal instructions

--- 

# Task 3 — Variables: User Information Script

## Goal

Use variables to store and display user-related information.

### Evidence

#### Screenshot 1 — Content of `user-info.sh`

![task 3](screenshots/Screenshot%20with%20content2_userinfo.png)

--- 

#### Screenshot 2 — Output of `./user-info.sh`

![task 3](screenshots/Screenshot%20with%20content2_output_userinfo.png)

--- 

### Notes

Answer the following in your own words:

**1. What is a variable in Bash?**

A Bash variable is a container which has temporary storage and holds data, such as a string of text or a number, which can be referenced  throughout a command-line session 

--- 

**2. Why should we avoid spaces around the `=` sign when creating variables?**

Because in bash failing to remove spaces causes the compiler or shell interpreter to misread the assignment as a command execution or a syntax error.

 The shell reads whitespace as a delimiter to separate commands and arguments hence when a command is typed with spaces in between it often is causes the command confusion 

 for example our_car = 18 is wrong
             our_car=18 is correct 
             (remove the whitespaces write the variables continously no space in between)

--- 

**3. How do you access the value stored inside a Bash variable?**

To access the value stored inside a Bash variable, you must precede the variable name with the sign $ e.g $index. the shell then substitutes the variable name with its actual content before executing the command
--- 

# Task 4 — Arrays & Loops: Tools Checklist Script

## Goal

Use arrays and loops to print a checklist of tools used in Bash scripting.

### Evidence

#### Screenshot 1 — Content of `tools-checklist.sh`

![task 4](screenshots/Screenshot%20with%20content3%20nano_Gnu.png)

--- 

#### Screenshot 2 — Output of `./tools-checklist.sh`

![task 4](screenshots/Screenshot%20with%20content3%20output%204_s.png)

--- 

### Notes

Answer the following in your own words:

**1. What is an array in Bash?**

An array in Bash is a   collection of variables bundle into a organized container which allows you to store variety of values under a one single variable name instead of managing separate variables like a list of file names, server IPs, or user choices.

--- 

**2. Why are arrays useful in scripts?**

Arrays are basic scripting features used to store several values in a single variable. They help to process large datasets quickly using loops and  reduce code repetition by eliminating the need to declare dozens of individual variables.

--- 

**3. What does `"${tools[@]}"` mean?**

"${tools[@]}"  ia a structured container of all the values stored inside the tools array. 

Here, it gives the loop access to every index position in the array.

The double quotes ensures each array item is a separate value.

---  


**4. What is the purpose of the `for` loop in this script?**

The purpose of a for loop in the script is to repeat a specific block of code/array a set number of times or iterate over the collection of varaibles sequentialy. It automates repetitive tasks, keeping the script clean and preventing reptition.

--- 

# Task 5 — Loops: Number Counter Script

## Goal

Use loops to repeat a task multiple times.

### Evidence

#### Screenshot 1 — Content of `counter.sh`

![task 5](screenshots/Screenshot%20with%20content%20of%20num_counter.png)

--- 

#### Screenshot 2 — Output of `./counter.sh`

![task 5](screenshots/Screenshot%20with%20output%205%20counter_sh.png)

--- 

### Notes

Answer the following in your own words:

**1. What is a loop?**

A sequence of instructions that automatically repeats itself until a specific condition is met

--- 

**2. Why do we use loops in Bash scripting?**

Basucally to process e files or arrays, automate repetitive tasks, , and control script execution dynamically without writing duplicate code. 

 a loop allows you to write the command once and let the system execute it as many times as necessary instead of typing the same command multiple times,.


--- 
**3. How many times did the loop run in your script?**

It iterated 5 times because we supplied it 5 values i.e. 12345

---

**4. What would you change if you wanted the loop to run 10 times?**

To allow the loop run exactly 10 times, you need to change its  its iteration range  between the starting point and the ending point is exactly 10 points. i.e. add 6 to 10 to the initial 1-5 to make the for loop:

for number in 1 2 3 4 5 6 7 8 9 10
do
   	echo "No $number "
done

--- 


# Task 6 — Files & Conditionals: File Validation Script

## Goal

Use file checks and conditionals to verify whether files and directories exist.

### Evidence

#### Screenshot 1 — Output of `ls -lah ../test-folder`

![task 6](screenshots/Screenshot%20with%20la_lah_test_folder.png)

--- 

#### Screenshot 2 — Content of `file-check.sh`

![task 6](screenshots/Screenshot%20with%20ls_lah_test_folder.png)

--- 

#### Screenshot 3 — Output of `./file-check.sh`

![task 6](screenshots/Screenshot%20with%20file_check_sh.png)
--- 

### Notes

Answer the following in your own words:

**1. What does `-d` check in Bash?**

Its a file test operator used within conditional statements to check if a specified path exists and is a directory

--- 

**2. What does `-f` check in Bash?**

Its an operator that checks if a target path exists and is a regular file. It evaluates to true only if both conditions are met

--- 

**3. Why should file and directory paths be stored in variables?**

It helps prevents code duplication, maintainability, readability, and ensures cross-platform portability. 

--- 

**4. What happens if the file does not exist?**

the line/lines of command under else will run, and the message displayed would be:
File does not exist: ../test-folder/....

---  


# Task 7 — Conditionals: Pass or Retry Script

## Goal

Use if-else conditionals to make decisions based on a variable value.

### Evidence

#### Screenshot 1 — Content of `score-check.sh` with `score=85`

![task 7](screenshots/Screenshot%20with%20score_check.png)

--- 

#### Screenshot 2 — Output showing `Result: Pass`

![task 7](screenshots/Screenshot%20with%20%20score_check_85_pt.png)
--- 

#### Screenshot 3 — Content of `score-check.sh` with `score=55`

![task 7](screenshots/Screenshot%20with%20content%20of%20score%20check%2055.png)

---  

#### Screenshot 4 — Output showing `Result: Retry`

![task 7](screenshots/Screenshot%20with%20output%20score%20check%2055.png)
--- 

### Notes

Answer the following in your own words:

**1. What is the purpose of if-else in Bash?**

if-else in Bash is conditional statement used to control the function of a script. 
It does this by executing different code segments based on whether a particular condition evaluates true or false. 

---  

**2. What does `-ge` mean?**

-ge typically means "greater than or equal to". In our ptoject its used in the bash script,
 it does a check whether the score is greater than or equal to 70

--- 

**3. Why should conditions be tested with different values?**

Beacuse it ensures system reliability, catches edge-case logic errors, and verifies that a system handles both standard and unexpected inputs safely.

--- 

**4. How can conditionals help in automation scripts?**

It helps script to dynamically make decisions and adapt to real-time inputs, rather than following a rigid, single-path sequence. 
They allow scripts to respond to errors, process different data types, and determine the next required action based on whether a specific condition is evaluated as true or false.

---  

# Task 8 — Functions: Final Bash Automation Script

## Goal

Create a final Bash script using functions to organize reusable code.

### Evidence

#### Screenshot 1 — Content of `final-automation.sh`

![task 8](screenshots/Screenshot%20with%20final%20automation.png)
--- 

#### Screenshot 2 — Output of `./final-automation.sh`

![task 8](screenshots/Screenshot%20with%20final%20automation%20printout.png)

--- 

#### Screenshot 3 — Output of `ls -lah` showing all created scripts

![task 8](screenshots/Screenshot%20with%20final%20final%20output.png)

--- 

### Notes

Answer the following in your own words:

**1. What is a function in Bash?**

Its reusable segment of commands that executes within the current shell environment

--- 
     

**2. Why are functions useful in scripts?**

they completely remove code repetition. They essentially act as features within your main script to make your code easier to write, read, and maintain

--- 

**3. Which functions did you create in this script?**

check_files
    print_header
    print_tools 
    print_user_details : prints full name and the assignment name.

--- 
    

**4. How does this final script combine variables, arrays, loops, conditionals, files, and functions?**

The final script uses variables to store values such as  the assignment name, paths.
An array is effective for this to store the tool names 
 A loop to is needed to print the values one by one.
It uses conditionals to check the required directory and file. 
The commands are seperated into several functions, and when compiled those functions are called to run the complete script.

--- 


# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

https://www.linkedin.com/feed/update/urn:li:activity:7484006478039818240/

---

#### Screenshot — Published LinkedIn post

[linkedIn](screenshots/Screenshot%20with%20automation%20script.png)

--- 

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- All script files must be created and run successfully
- Required notes must be answered clearly for every task
- Do not expose sensitive information (keys, passwords, credentials)

---

# Completion Checklist

- [✅ ] Task 1: Environment setup verified, workspace created (Screenshots 1–2, Notes answered)
- [✅ ] Task 2: First script created, executed, permissions verified (Screenshots 1–3, Notes answered)
- [✅ ] Task 3: Variables script created and run (Screenshots 1–2, Notes answered)
- [✅ ] Task 4: Arrays and loops script created and run (Screenshots 1–2, Notes answered)
- [✅ ] Task 5: Counter loop script created and run (Screenshots 1–2, Notes answered)
- [✅ ] Task 6: File validation script created and run (Screenshots 1–3, Notes answered)
- [✅ ] Task 7: Pass/Retry conditional script tested with both values (Screenshots 1–4, Notes answered)
- [✅ ] Task 8: Final automation script created and run (Screenshots 1–3, Notes answered)
- [✅ ] All scripts run without errors
- [✅ ] Full Name visible in all required screenshots
- [✅ ] LinkedIn post published and URL submitted
- [✅ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*