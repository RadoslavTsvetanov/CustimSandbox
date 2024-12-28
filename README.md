# CustimSandbox




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


#### How it works

##### Syscall injection
![image](https://github.com/user-attachments/assets/02f8606c-0e42-4f91-ba3a-2db5f474aa5b)
Each syscall onvoked is being provided as a wrapper aroundd the original function function to which you have acces to the params and can do whatever you want 
