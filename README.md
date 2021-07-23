# auto-deploy-to-digitalocean
Automatic deployment of your site or application to any VPS, in my case it is DigitalOcecan

# Step 1: Generate an SSH Key
In this case we’ll SSH into the server.

    ssh username@host.com

Once you’re in the server, navigate to the .ssh folder. We will generate the SSH key here.

    cd ~/.ssh

So here’s the command you need to use. Remember to replace your_email@example.com with your email address.

    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

Next we need to name the SSH Key file. I recommend switching the file name to github-actions so we know this key is used for Github Actions. It pays to be explicit when you view your SSH keys 6 months down the road.


<img src="https://zellwk.com/images/2021/github-actions-deploy/name-ssh-key-file.png" />

If you use the ls command now, you should see your keys in the .ssh folder.

    ls

The public key contains a .pub extension while the private key doesn’t.

<img src="https://zellwk.com/images/2021/github-actions-deploy/public-key-extension.png" />

# Step 2: Adding the Public Key to authorized_keys

We need to add the public key (github-actions.pub) to authorized_keys so machines using the private key (github-actions) can access the server.
The easiest way is to use a cat command to append github-actions.pub into authorized_keys. It look like this:

    cat github-actions.pub >> authorized_keys

# Step 3: Adding the private key to your repository’s secrets

Go to your repository on Github and click on “Settings”, then “Secrets”. You should see a button that says “New repository secret”.

<img src="https://zellwk.com/images/2021/github-actions-deploy/github-secrets-location.png" />

<img src="https://zellwk.com/images/2021/github-actions-deploy/new-repository-secret-button.png" />

Click “New repository secret” and you’ll be prompted to enter a secret. This secret contains two things — a secret name and the contents. The secret name is used to get the contents later in a Github Actions workflow.
In my case, I chose to name the secret SSH_KEY.

For the value, we need to go back into your server and open up the github-actions private key. We can do this with nano.

    nano github-actions

You’ll see a file similar to this.

<img src="https://zellwk.com/images/2021/github-actions-deploy/private-key.png" />

# Step 4: Adding a correct host value

<img src="https://clustercs.com/kb/wp-content/uploads/2018/11/DO11.png" />

Manually add the IP address (which I named as SSH_HOST) into the Github Secrets.
