# Assignment1

# Tutorial: Setting Up an Arch Linux Server on DigitalOcean (Level 2)

## Prerequisites
Before starting, ensure you:
- Have a DigitalOcean account.

## Step 1: Generate SSH Keys
SSH keys provide a more secure way to connect to your server than using a password. Follow these steps to generate SSH keys:

### 1.1 Generate SSH Keys on Your Local Machine
Open your terminal and run the following command to generate SSH keys:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "your_email@examplemail.com"
```

The command above generates two plaintext files
* "do-key" the private key
* "do-key.pub" the public key that needs to be copied to the server

### 1.2 Add the SSH Key to Your DigitalOcean Account
To add the newly created SSH key to your DigitalOcean account, follow these steps:

1. Log in to DigitalOcean.
2. Click **Settings** on the dashboard.
3. Navigate to the **Security** section in the dashboard.
4. Click on **Add SSH Key**.
5. Copy the content of your public key (found in `~/.ssh/do-key.pub`).
6. Paste the key into the provided field and give it a name.
7. Click **Add SSH Key**.

## Step 2: Add a Custom Arch Linux Image
Next, you will add a custom Arch Linux image to your DigitalOcean account. This is the Operating System the droplet will use.

### 2.1 Download the Arch Linux Image
1. Visit the [Arch Linux download page](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/).
2. Select and download the latest version of the Arch Linux-cloudimg .qcow file.
![screenshot4](/images/screenshot4.jpg)

### 2.2 Upload the Image to DigitalOcean
1. In your DigitalOcean dashboard, navigate to the **Backups & Snapshots** section.
2. Click on **Custom Images**.
3. Select **Upload Image** and follow the prompts to upload your Arch Linux ISO.
![screenshot3](/images/screenshot3.jpg)

## Step 3: Set up a Droplet Running Arch Linux
Now that you have your custom image uploaded, you can create a Droplet. Droplets are Linux based VMs that run on virtualized hardware. (DigitalOcean Droplets)

### 3.1 Create the Droplet
1. Go to the **Droplets** section in your DigitalOcean dashboard.
2. Click **Create Droplet**.
3. Choose the **Arch Linux** image from the **Custom Images** tab.
4. Select the plan, data center region, and additional options as needed.
5. Under the **Authentication** section, choose **SSH Keys** and select the key you added earlier.


## Step 4: Configure Cloud-Init
To automate the initial setup tasks, you will create a cloud-init configuration file.

### 4.1 Create the Cloud-Init Configuration File
Create a new file named `cloud-config.yml` with the following content:

```yaml
#cloud-config
users:
  - name: newuser
    ssh-authorized-keys:
      - ssh-ed25519 your_key_here
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    shell: /bin/bash
packages:
  - vim
  - git
disable_root: true
```

### 4.2 Add Cloud-Init to the Droplet
1. When creating the Droplet, find the **Advanced Options** field.
2. Click **Add Initialization Scripts**
3. Place your `cloud-config.yml` content into this field.
![screenshot2](/images/screenshot2.jpg)

### 4.3 Finalize the Droplet
Click **Create Droplet**.
Once finished it should look something like this
![screenshot](/images/screenshot1.jpg)

## Step 5: Connect to Your Droplet Using SSH
After your Droplet is created, connect to it using SSH.

### 5.1 Find the droplet IP
1. Go to the Droplets section in your DigitalOcean dashboard.
2. Locate your new Droplet and note its public IP address.

### 5.2 Connect via SSH
In your terminal, run the following command to connect to your Droplet:

```bash
ssh -i .ssh/do-key arch@your_droplet_ip
```

Replace your_droplet_ip with the actual IP address of your Droplet

## References
DigitalOcean droplets: Scalable cloud compute starting at $4/MO. DigitalOcean Droplets | Scalable Cloud Compute Starting at $4/mo. (n.d.). https://www.digitalocean.com/products/droplets 