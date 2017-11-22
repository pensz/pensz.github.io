# How install Tensorflow on Ubuntu 16.04


## Step 1: Install Python 3

Update your repository list first:

    apt-get update

Then, install Python 3, Python 3 have better performance than Python 2:

apt-get install python3-pip python3-dev python-virtualenv

## Step 2: Setup Python 3 env for tensorflow

Setup a separated env for tensorflow

    virtualenv --system-site-packages -p python3 ~/tensorflow

    source ~/tensorflow/bin/activate

Update pip first to make sure pip >= 8.1

    pip install --upgrade pip


## Step 3: Confirming the installation of tensorflow

Install tensorflow without GPU version:

    pip install --upgrade tensorflow

## Step 4: Confirming the installation of tensorflow

Create a example python to test

    vi /tmp/tf-example.py

Enter code as following ：

    import tensorflow as tf
    hello = tf.constant('Hello, Tensorflow!')
    print(tf.Session().run(hello))

Run it :

    source ~/tensorflow/bin/activate
    python /tmp/tf-example.py


You will now see a output like:

    b'Hello, Tensorflow!'

Congrats! You have now installed Tensorflow!.
