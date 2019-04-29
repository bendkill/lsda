# Earth to Jupyter in 30 Seconds: Getting Started on Midway

So, you need some serious compute power? Great! If you're in Large Scale Data
Analysis (LSDA), the university Research Computing Center (RCC) and its Midway
compute cluster **are here for you!** They have lots of great documentation and
guides [here](https://rcc.uchicago.edu/docs/using-midway/index.html), and those
docs are awesome! This guide is intended to take you from knowing nothing about
Midway to using a jupyter notebook for LSDA.

Benefits of using Midway:
* **Power-efficient**: working remotely uses less battery.
* **Space-efficient**: big data is **big**. Don't keep it on your laptop!
* **Fast**: Midway has high-performance CPUs and GPUs. Code that can take an hour
  on my laptop runs in seconds on Midway.
  
Drawbacks of using Midway:
* You, um, need a decent internet connection. Fortunately, this is 2019.
* Starting an interactive session can take a few seconds.

Let's get started!

## Step 0: Two Factor Authentication

Midway requires two-factor authentication (2FA). Set it up
[here](https://cnet.uchicago.edu/2FA/index.htm) if needed.

## Step 1: SSH into Midway

To log into Midway, open a terminal and run
```
ssh <CNetID>@midway2.rcc.uchicago.edu
```

You will be prompted for your CNet password. After entering it, you should see
something like
```
Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-1234
 2. Phone call to XXX-XXX-1234
 3. SMS passcodes to XXX-XXX-1234

Passcode or option (1-3):
```

Type `1`, `2`, or `3`, and hit enter. Once you have confirmed the 2FA prompt,
you will be logged into Midway on a login node.

## Step 2: Navigate to a Working Directory

Navigate to the directory you want to work in. Your home directory
`/users/<CNetID>` is *yours*. You can create a directory for this class here,
using `mkdir lsda`. Navigate to it with `cd lsda`.

## Step 3: Open an Interactive Node

On Midway, there are *compute nodes* and *login nodes*. A good analogy is
putting on shoes. When you log in, you are on a login node, which is like being
barefoot. You probably won't do much on a login node, but you can look around.

Compute nodes are like work boots; you use them to get stuff done. In LSDA, we
will be using interactive nodes, which are a type of compute node. To start an
interactive node, run
```
sinteractive --nodes=1 --ntasks=10 --account=cmsc25025 --time=4:00:00 --mem=4000
```
which will give you 4 hours to work with 4 GB of RAM. Feel free to change the
session length. The memory allotment should be sufficient for the homeworks.

## Step 4: Start a Notebook

To start a jupyter notebook, run
```
/project/cmsc25025/run_ipython.sh Anaconda3/2018.12
```

This may take a few seconds to get started, but eventually you will see
something like:
```
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://10.50.221.191:21926/?token=9c904f4f72506d2862162a5e7606202a5a89ff18938d1736
```
which you can copy into a browser (Chrome, Safari, etc.) to open the jupyter
notebook. 

If you use [iTerm](https://www.iterm2.com/) on a Mac, you can hold down command
and click on the link, which I always find a little less annoying than
copy-pasting.

That's it! You've now started a jupyter notebook and can work on the coding
sections of homework.

## Now, wasn't that annoying?

Instead of typing or copying all that in every time you want to do the homework,
you should use
[this](https://github.com/bendkill/lsda/blob/master/scripts/midway) little
script that does it for you!

It requires some setup. From a directory on your PATH, run:
```
curl https://raw.githubusercontent.com/bendkill/lsda/master/scripts/midway -o midway
chmod +x midway
nano midway
```
and use `nano` (or your favorite text editor) to replace
`<CNetID>` with your CNetID and `YOUR_PASSWORD` with your password. You may also
want to change `1` to your preferred 2FA method. Afterward, you can log in to
Midway and start a jupyter notebook simply by running
```
midway
```
from any terminal on your computer. It should take about thirty seconds.

### You should note:

1. This script requires storing your CNetID password in plaintext on your
   machine, which is a minor security risk if your disk is not encrypted. If you
   would rather enter your password each time, remove lines 3 and 4 from the
   script.
2. This security concern is somewhated mitigated by 2FA (yay!), which still
   requires you to confirm the login using your phone or other secondary device
   (ugh). The script will wait while you do this.
3. I have a Mac. These instructions should also work on Linux. If you have a PC,
   they should work inside the Unix environment, but this has not been tested.

## Other Useful Items

### Copying Data

You can copy data to your local machine using `scp`. For example, 
```
scp <CNetID>@midway2.rcc.uchicago.edu:/project/cmsc25025/mnist/MNIST.npy .
```
which will copy the MNIST data to your current directory (after password and 2FA
prompts). To get the labels, run:
```
scp <CNetID>@midway2.rcc.uchicago.edu:/project/cmsc25025/mnist/MNIST_labels.npy .
```

### Loading Packages

On Midway, programs or "modules" are loaded using the `module load <module>`
command, where `<module>` is the desired module, e.g. `python`. For our
purposes, the packages loaded by `run_ipython.sh` should be sufficient, but you
can read more about them
[here](https://rcc.uchicago.edu/documentation/_build/html/tutorials/intro-to-software-modules/index.html).

If you need to install Python packages, this can be done using `pip`. First,
start an interactive session and load your desired Python environment. If you
used the instructions above, this should be
```
module load Anaconda3/2018.12
```

Once the Python module is loaded, you can run
```
pip install --user <package>
```
to install `<package` (e.g. numpy, pandas) in `user/<CNetID>/.local`.
