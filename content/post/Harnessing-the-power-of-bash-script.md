---
title: "Harnessing the Power of Bash Script"
date: 2024-01-09T23:33:22+05:00
---

This is a short, fun blog that demonstrates how we can use basic bash commands to make our life easier and flex on fellow developers.

<!--more-->

So, what's a bash script, bash is command-line language which helps you interact with operating system through command-line-interface (Terminal), in other words, whatever you can do with nice graphical interface you can do by just running _some_ commands in terminal.

Script is anything (.txt, .py file etc) from which a program reads (interprets) an instruction, executes it and then move to the next instruction. Thus, `Bash Script` is a script written in bash which is interpreted by the Bash (interpreter) enabling us to interact with the OS. Yes, bash is the name of the language as well as an interpreter.

I believe that's enough background information. Coming back to main topic, I've been working on a project for last few months for which I need to launch three applications. A Browser (chrome), code-editor (vscode) and `disabled-web-security` instance of my browser. Let me demonstrate you how can we open these three applications using single command `sw`.

We can open all three applications separately by running `google-chrome &` for chrome, `code ~/target/dir` for vscode and `google-chrome --user-data-dir=insecure-chrome --disable-web-security &` for chrome with reduced security. `Ampersand &` is used for exceuting commands asynchronously, allowing us to execute the next command without waiting for former to finish.

We can combine these three commands into a single command by typing them all out and separating them with semi-colon. `Semi-colon` tells bash that a command ends here and a new command will start after it, it follows in-order execution. We used parathensis to separate `&` from `;`

```bash
(google-chrome &) ; code ~/target/dir ; (google-chrome --user-data-dir=insecure-chrome --disable-web-security &)
```

If you followed the above steps, you'll notice that it displays bunch of warnings/errors after we run this command, we can deal with them later. But the big worry is, do we need to type all this when we want to setup the workspace? hmm, well no.

We can leverage concept of aliasing in bash through which we can assign custom names to different commnads. We can open the .bashrc, or .zshrc if you're on mac, using `sudo nano ~/.bashrc`. It's the main bash file which is run everytime we launch a new terminal instance.

Here we will add a alias named `sw` for the above command

```bash
alias sw="(google-chrome &) ; code ~/target/dir ; (google-chrome --user-data-dir=insecure-chrome --disable-web-security &)"
```

We will run `source ~/.bashrc` to use the updated bashrc for current terminal instance. As I mentioned above, we don't get a clear terminal automatically after these instructions are executed. We can get one by adding three more commands to our alias, updated alias will look something like this.

```bash
alias sw="(google-chrome &); code ~/target/dir ; (google-chrome --user-data-dir=insecure-chrome --disable-web-security &) ;sleep 1 ; kill -TERM $$ ; clear"
```

`sleep 1` will wait for instructions to be executed as most of them are executed asychronously. `kill -TERM $$` will signal an interept, which will terminate currently running process(es), and finally `clear` will clear the terminal window.

There's last edge case we're missing, what if a chrome instance is already up and running then the above command will open a new chrome instance which we will have to close manually. A simple work around would be to create a function in `.bashrc` that checks if the there's a chrome instance already running, if yes, we don't need to launch a new one.

```bash
checkChrome() {
    if ! pgrep -x "chrome" > /dev/nul ; then
        google-chrome &
    fi
}
```

Above function uses grep, a command-line search utility, to find a process named `chrome`, if it isn't present then it launches a new one. `>` redirects the output of the command to `/dev/null` file which is a system-file and acts as black hole, any data written to it is discarded and not stored anywhere. 

How can we call this function in our alias? simply replace the `google-chrome &` command with `(checkChrome)`. That's how we can launch three apps with just one simple command `sw` and it justs tip of the ice berg. I haven't used a single complex command but still able to do alot, that just demonstartes how powerful bash script is and how important it is to get familiar with it. 

Idea of setting up workspace like above came from [this](https://www.youtube.com/watch?v=EfmVRQjoNcY) scene in ironman, although it's not that good but give the same vibes. My dream setup goal is to build something like [this](https://www.youtube.com/watch?v=RjF_j6QZpxc) but that's only possible if I get a good job and save enough money to afford the required hardware so please pray for my job.