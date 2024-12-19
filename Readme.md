docker build -t my-jupyter .

docker run -p 8888:8888 my-jupyter

# Step 1: Install Docker

Before you start, make sure you have Docker installed on your machine. If you haven't installed Docker yet, you can download it from the [official Docker website](https://www.docker.com/get-started).

# Step 2: Project configuration

Create a directory dedicated to your Jupyter Notebook project. In this directory, configure the following essential files.

```
mkdir directoryname
```

## Step 2.1: Dockerfile

Create a file called "Dockerfile" and add the following content:

```shell
# Use an official Python runtime as a parent image
FROM python:3.8

# Set the working directory to /app
WORKDIR /app

# Install Jupyter Notebook
RUN pip install jupyter

# Make port 8888 available to the world outside this container
EXPOSE 8888

# Define environment variable
ENV NAME World

# Run Jupyter Notebook when the container launches
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
```

## Step 2.2: requirements.txt (optional)

If your project has dependencies, create a "requirements.txt" file listing them.

```shell
# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
```

- `FROM python:3.8` - here we're using a rather large image of Python. I could have used a lighter image like an alpine or a buster-slim. I opted for simplicity and decided to use the default image.
- `RUN pip install -R` requirements.txt - we install the dependencies. The most important (and also the heaviest) is TensorFlow. I'll come back to this later.

you can add this code to dockerfile and create a requirement.txt file
file if you have requirements.

# Step 3: Create a Docker image

Open a terminal, access the project directory and run the following command to create the Docker image:

```shell
systemctl start docker
docker build -t mon -jupyter .
```

# Step 4: Run the Docker container

Once the image has been created, run the following command to execute a Docker container:

```shell
docker run - p  8888 : 8888 mon-jupyter
```

This command maps port 8888 on your host machine to port 8888 on the Docker container.

# Step 5: Access Jupyter Notebook

Open your web browser and go to [ http://localhost:8888]

instead of "localhost", you must paste your public IP to access the Jupyter Notebook

You can find your IP address using these codes:

```shell
ifconfig (linux)
ipconfig (windows)
```

now, when you search for [ http://65.0.169.235:8888], you will arrive at your Jupyter login page

Now, if you return to your environment, you will find the connection token for your Jupyter notebook.

# Instructions for sending a Docker image

## Method 1: Use Docker Hub

### 1. create an account on Docker Hub

Create an account on [Docker Hub](https://hub.docker.com/).

### 2. Connect to Docker Hub from your terminal

Use the following command to connect to your Docker Hub account:

```sh
docker login
```

### 3. Taguer votre image Docker

Before pushing your Docker image to Docker Hub, you need to tag it with your Docker Hub username and the name of the repository. For example, if your username is `username` and you want to name the repository `my-jupyter-notebook`, use the following command:

```sh
docker tag my-jupyter-notebook username/my-jupyter-notebook
```

### 4. Pushing the image to Docker Hub

Use the following command to push the tagged image to Docker Hub:

```sh
docker push username/my-jupyter-notebook
```
## Method 2: Using a tar file

### 1. Save the Docker image as a tar file

Use the `docker save` command to save the Docker image as a tar file:

```sh
docker save -o my-jupyter-notebook.tar my-jupyter-notebook
```

### 2. Transfer the tar file

Transfer the tar file to the other machine using tools such as `scp`, `rsync`, `ftp`, or even using a USB key. For example, with `scp` :

```sh
scp my-jupyter-notebook.tar user@destination_machine:/path/to/destination
```

Replace `user` with your user name and `destination_machine` by the address of the destination machine.
### 3. Load the Docker image from the tar file

On the destination machine, use the command `docker load` to load the Docker image from the tar file :

```sh
docker load -i /path/to/destination/my-jupyter-notebook.tar
```

## Full example

### On the source machine

1. Tag the Docker image :

   ```sh
   docker tag my-jupyter-notebook username/my-jupyter-notebook
   ```

2. Push the image to Docker Hub :

   ```sh
   docker push username/my-jupyter-notebook
   ```

or

1. Save the Docker image as a tar file :

   ```sh
   docker save -o my-jupyter-notebook.tar my-jupyter-notebook
   ```

2. Transfer the tar file :

   ```sh
   scp my-jupyter-notebook.tar user@destination_machine:/path/to/destination
   ```

### On the destination machine

1. Load the Docker image from the tar file :

   ```sh
   docker load -i /path/to/destination/my-jupyter-notebook.tar
   ```
