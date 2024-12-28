# CustimSandbox

## Supported OSes
- Linux: yes
- Windows: pff did you think i was gonna say yes, i am not developing for this shit
- MacOS: if someone buys me a mac for testing it i will try my best


## Why
Ever used a sandbox soltion>?
- No ?
Well i have and they are a pain to work with and configure and since i have too much free time i decided to make one

## Features
### syscall injections
All the modern codes works thanks to syscalls and so every exploit needs to use them so what better and more robust way to stop code than modifying the syscalls themselfs

There are two ways to write syscall injections
- modifying the syscall directly


#### Modyfying the Syscall directly 
Since the sandbox works by injecting a wrapper aound the syscall nothing stops you from modifying the code directly 

This allows for some graceful methods to protect our data, here is a script which if a netwrok request is made to any place outisde a certain domain it faIls

```
func custom_syscall(req){ // this is an imaginary syscall, note that this will be inlined inside the actual sycall code 
  if(!req.url.conatins("hi.com")){
    return (1)
}
}
```
using this even if someone compromises the server using command injection since he is running in the sandbox there is no way for him to send our data anywhere (yupieee)

### Binary injections
Another thing that is common for attackers to use are built in linux binaries like curl, ping, ls, rm etc...
So a good way to defend is to just not give them to the attacker but that on a normal machine is hard close to impossible however our sandbox works as a proxy between everything that the 
program needs to get from the host so just as sycalls we can also modify the binaries as well

## Where is it useful?
if you first thing about it you will think to yourself `pff this id bs there is so much overenginnering i will just use a lib for blacklisting` 
and yeah this is a valid opinion but your blacklist lib could be vulnerable to one of the mmany types of input sanitization bypasses whic hcould be used for command injection but they wont go through the sandbox. 

Imagine the following sceanrio 
You have created a input which executes a command but you have balcklisted `ls` but the lib you have chosen hasnt implemented character shifting defense so the string is evaluated to ls (now if you are running without my sandbox this is the end), lucky for you you are in the sandbox and you have made the ls binary to point to nothing so even if he excutes ls nothing happens since it calls an empty binary 

## How it works
### In general
Bassically it creates a vitual space a (a light vm even) in which it overwrites the syscalls and for the binaries it just created a sym link to the custom binary in placve of the prignal one executable


### Syscall injection
![image](https://github.com/user-attachments/assets/02f8606c-0e42-4f91-ba3a-2db5f474aa5b)
Each syscall onvoked is being provided as a wrapper aroundd the original function function to which you have acces to the params and can do whatever you want 

### Binary injection
In the virtual space we have created we just change thee PATH



## Docs 
CustimSandbox uses projects to resonate between different sandbox contexts so to create a new project go in the projects folder and create a directory with whatever name you want. In there you need to create a config file which follows the 










## Resources Dump (here are materials which are images files etc... that need to be refernced multiple times or just to be a bit cleaner)

### Config Template
{
syscalls: CustonSyscall[]
binaries: CustomBinary[]
}


### Types
#### Custom Syscall
```json
{
syscall_id: string,
content: string
}
```

example one 
```json
{
  content: ```
  if(req.url.containes("google.com")){
    exit())
  }
}
```

#### Cusomt Binary
```json
{
"id":string,
"path_to_custom_binary": string
}
```

example

```json
"id":"curl",
"path_to_custom_binary":"/home/custimCurl"
```
