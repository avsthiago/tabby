# Steps to setup the project

- Fork the repo
- Clone the repo
- Follow the contribution guidelines [link](https://github.com/avsthiago/tabby?tab=readme-ov-file#-contributing)
- cargo build 
- cargo run # runs the application locally
- docker build -t tabby . # builds the docker image
- docker run -it -p 8080:8080 -v $HOME/.tabby:/data tabby serve --model TabbyML/StarCoder-1B
- Find all **/v1/health** endpoints inside `crates` and replace them with **/ping**
- Find all **/v1/completions** endpoints inside `crates` and replace them with **/invocations**
- create the serve file

```
#!/bin/bash

# Replace <bucket_name> with your actual bucket name
# aws s3 sync s3://<bucket_name>/tabby /data
#ls -lah .
# Run the tabby command
/opt/tabby/bin/tabby serve --model TabbyML/StarCoder-1B --device cpu
```
- Update the docker file to include the serve file and the aws cli command to sync the s3 bucket
- At this point you can run the docker image locally and test it
```
docker run -it -p 8080:8080 -v $HOME/.tabby:/data <image_name> 
```
- Run the docker image at least once to download the model under $HOME/.tabby
- Copy everything inside $HOME/.tabby to the s3 bucket
```
 aws s3 cp ~/.tabby s3://<your_bucket>/tabby/ --recursive 
```
- read this https://sagemaker-workshop.com/custom/containers.html
- you can find more information about the instances at https://aws.amazon.com/sagemaker/pricing/, the price is not the same as the regular ec2 instances


```
docker run -it -p 8080:8080 -v $HOME/.tabby:/data 24ca0d42abb18e9cfd2f229

curl -X 'POST' \                     
  'http://localhost:8080/invocations' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "language": "python",
  "segments": {
    "prefix": "def fib(n):\n    ",
    "suffix": "\n        return fib(n - 1) + fib(n - 2)"
  }
}'
{"id":"cmpl-c1592b04-7ef5-491e-a82f-495386596145","choices":[{"index":0,"text":"    if n <= 1:\n            return n"}]}%



curl -X 'POST' \                        base
  'https://08rjcvlje8.execute-api.eu-west-1.amazonaws.com/v1/generate' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "language": "python",
  "segments": {
    "prefix": "def fib(n):\n    ",
    "suffix": "\n        return fib(n - 1) + fib(n - 2)"
  }
}'
{"id":"cmpl-3841a109-17d6-4223-99bb-9595454b2777","choices":[{"index":0,"text":"    if n <= 1:\n            return n"}]}%
```