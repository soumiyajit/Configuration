1. Install curl if not installed
   - apt install curl

2. Download the latest version of go as below:
   - curl -OL https://go.dev/dl/go1.23.3.linux-amd64.tar.gz

3. verify the integrity of the file using the command below:
   - sha256sum go1.23.3.linux-amd64.tar.gz

4. use tar to extract the tarball
   - tar -C /usr/local -xvf go1.23.3.

5. add the following line to the end of the profile file
   - export PATH=$PATH:/usr/local/go/bin
   [ vi ~/.profile and add the line ]
6. Refresh the profile by running the following command:
   - source ~/.profile

7. check the release of the go package installed:
   - go version


