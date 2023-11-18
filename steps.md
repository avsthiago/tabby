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