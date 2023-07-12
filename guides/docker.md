# Docker

Docker is one of the ways that you can run the code directly from the github page.

## Installing Docker

Here are the commands that you will need to run in Ubuntu's terminal in order to install everything
```
sudo apt-get update

sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

After you have finished running all these commands, you should be able to run 
```
docker -v
```
which will give you a message that looks like: Docker version 20.10.14, build a224086349

## Running a DeepHyperX Docker image

Now that Docker is installed, we can build the DeepHyperX Docker image.

In your terminal run this command:
```
sudo docker build -t deep-hyper-x https://github.com/nshaud/DeepHyperX.git
```
-t allows us to set a tag/name for the image.

Now that the image is created, run it with 
```
sudo docker run -p 9999:8097 -ti --rm deep-hyper-x
```

You will now be able to go to [localhost:9999](http://localhost:9999/) to check the results of the tests you do in this image.

To run a basic test go back to your terminal and put in the command:
```
python main.py --model SVM --dataset IndianPines --training_sample 0.3
```

Once it is finished you can go back to localhost and change the Environment from main to IndianPines SVM to check the results
