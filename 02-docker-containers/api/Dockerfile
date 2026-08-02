# Dockerfile 
# It is essentially the declaritive manifest that provides information on how a given container will run.
# It is the file where we declare wht our application needs



# Alpine Linux is an extremely small distribution.
# And this significantly reduces the size of the final image.
FROM node:18-alpine3.19

# Sets the working directory inside the container
WORKDIR /usr/src/app


# Copies files from your computer into the image.
COPY package.json package-lock.json ./

# Docker executes and installs all the dependencies. 
# node_modules is created within the image.
RUN npm install 

# Copies the rest of the project.(computer -> workdir)
COPY . . 


# Docker executes.
RUN npm run build

# My application is listen port 3000
EXPOSE 3000

# This command is executed when the container starts.
# CMD - defines the main process of the container.
CMD ["npm", "run", "start"]