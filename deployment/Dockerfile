#
# Build the docker image when invoked from the deployment script
#

# Use the Node docker image for the lightweight Alpine Linux OS
FROM node:14.15.1-alpine

# Install tools for troubleshooting purposes
RUN apk --no-cache add curl
RUN apk --no-cache add openssl

# Set the API folder
WORKDIR /usr/sampleapi

# Copy files into our docker image
COPY dist/                      /usr/sampleapi/dist/
COPY data/*                     /usr/sampleapi/data/
COPY deployment/api.config.json /usr/sampleapi/
COPY package*.json              /usr/sampleapi/
COPY tsconfig.json              /usr/sampleapi/

# Create a low privilege user, which will by default have read access to our files under /usr
RUN addgroup -g 1001 samplegroup
RUN adduser -u 1001 -G samplegroup -h /home/sampleuser -D sampleuser

# Install production dependencies
RUN npm install --production

# Configure the Linux OS to trust development root certificates
COPY deployment/rootCerts.pem /usr/local/share/ca-certificates
RUN update-ca-certificates
ENV NODE_EXTRA_CA_CERTS=/usr/local/share/ca-certificates/rootCerts.pem

# When a container is run with this image, run the API as the low privilege user
USER sampleuser
CMD ["npm", "run", "startRelease"]
