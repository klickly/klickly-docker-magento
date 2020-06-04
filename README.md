# klickly-docker-magento
Easy setup of Magento store with pre-installed klickly extension

### Steps to install on EC2 instance:
1. Run EC2 instance. Required to be ubuntu 18.04.
2. Allocate and attach Elastic IP to this instance
3. Add host name in Route 53 and map to this Elastic IP
4. Connect to EC2 instance by ssh
5. Clone this repo using command `git clone https://github.com/klickly/klickly-docker-magento.git && cd klickly-docker-magento`
6. In `.env` file change required variables
   - HOST_NAME - To setup SSL certificates
   - MAGENTO_URL - To set as base url in Magento
   - EXTENSION_BRANCH - Branch in klickly extension
7. Run `bash start`