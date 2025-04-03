# Creating you own terminal commands

Did you know that you can create your own custom commands to run in the terminal? Similar to how you use `cd`, `ls` and so on...

## Why bother? 

Sure you can live without it but once you get tired of repetitive commands you have to type in the terminal again and again you might appreciate the benefits if it. 

For example, every day I clone student's projects and have to install node modules and then start the app. 

If it's a client, I do:

```
npm i
npm start
```

Once i got tired of typing that i created my own command `istart` which executes both of the above mentioned commands and starts a client after installing all the node modules. 

## How? 

On most systems, there's a file like `.bash_profile`, `.bashrc`, or `.zshrc` depending on your shell in the root folder which can help us to do so. 

First we need to find this file on your system.

### On Mac

Go to the root folder by running `cd` without anything else in the terminal. 

Check with `pwd` if you are in the right place. The output is something like `/Users/YourUserName` where `YourUserName` is your username on your Mac. 

Run `ls -a` to see the hidden files and scroll to see if you have a file named `.bash_profile`. 

Note: macOS now uses the zsh shell by default, so instead of `.bash_profile`, you might want to use `.zshrc`. To check which shell you're using, run:
```
echo $SHELL
```

Open it in your code editor or create it if you do not have it. 

In this file you might see something like:
```
export PATH=/usr/local/bin:$PATH
export PATH=/Applications:$PATH
export REACT_APP_EDITOR=code
export PATH="$HOME/.composer/vendor/bin:$PATH"
```

### On Windows (with GitBash)

Locate or create your `.bashrc` file -- GitBash uses the `.bashrc` file to load custom configurations and aliases.

Open GitBash and navigate to your home directory by running:
```
cd ~
```

Check if the `.bashrc` file exists:
```
ls -a
```

If it's not there, you can create it with:
```
touch .bashrc
```

Open `.bashrc` in your code editor.

### Both systems: adding aliases

This is where we are going to create our custom commands. The format is:
```
alias YourCustomCommand="what to do for this command"
```

Let's test it by making a new line after all the code you already have in this file and adding this:
```
alias test="echo 'hello'"
```

Now save the file, go to the terminal and run `source ~/.bash_profile` for Mac or `source ~/.bashrc` for Windows for the changes to take effect. 

Try if it works by running `test` command which should print "hello" in the terminal. 

If it works -- success! 

Now for our case where we want to create `istart` which will install node modules and start the client app we will add a new line with the following code:
```
alias istart="npm i && npm start"
```

Since it executes 2 commands in sequence we can use logical && to run them one after another. 

Anoher case is for pushing to GitHub again and again:
```
alias pushgh="git add . && git commit -m "updates" && git push"
```

Now by running `pushgh` you will add the current folder to a commit and push it to GitHub. 

However, it will always push with the same commit description -- "updates". 

This is not ideal. If you want to do a step further you can use the shell scripts to make it dynamic. 

Create a new file called "autopush.sh" in the same root user's folder. Open it in code editor and put the following code there:
```
str="$*"
echo $str
git add .
git commit -m "$str"
git push
```

It will take a string argument for the commit's decription and use it in `git commit` command. 

Now back in you `.bash_profile` add an alias to run this script:
```
alias apush="sh ~/autopush.sh"
```

And now we can run `apush 'actual commit description'` which will use `sh` to run the shell script 'autopush.sh' and it will pass the 'actual commit description' to be used as a description of the current commit! 

Isn't it wonderful? 