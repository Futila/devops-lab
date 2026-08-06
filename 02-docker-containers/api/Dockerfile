# ------------------------------------------------------------------------------
# Dockerfile
#
# A Dockerfile is a declarative file that contains all the instructions
# Docker needs to build an image for our application.
#
# Think of it as a recipe:
#   - The Image is the final product.
#   - The Container is an instance created from that image.
#
# Docker executes each instruction from top to bottom, creating a new layer
# after every command. These layers are cached to speed up future builds.
# ------------------------------------------------------------------------------

# ------------------------------------------------------------------------------
# BUILD STAGE
#
# This first stage is responsible for compiling the application.
# It contains everything needed to build the project, including
# development dependencies like TypeScript, Nest CLI, ESLint, etc.
#
# This stage will NOT be part of the final image.
# ------------------------------------------------------------------------------

# Alpine Linux is a lightweight Linux distribution (~5 MB),
# making the final Docker image much smaller.
FROM node:18-alpine3.19 AS build

# Defines the default working directory inside the container.
# Every following command (COPY, RUN, CMD...) will be executed
# relative to this directory.
WORKDIR /usr/src/app

# Copy only the package files first.
#
# This is a Docker optimization:
# If package.json/package-lock.json do not change,
# Docker reuses the cached "npm ci" layer,
# avoiding reinstalling dependencies on every build.
COPY package.json package-lock.json ./

# Install ALL dependencies (including devDependencies).
#
# We need development packages because NestJS must compile
# the TypeScript source code into JavaScript.
RUN npm ci

# Copy the rest of the application's source code.
#
# Since dependencies are already installed,
# changing source files won't invalidate the npm install cache.
COPY . .

# Build the application.
#
# Executes:
# npm run build
#
# Which, in this project, runs:
# nest build
#
# The result is the "dist/" folder containing the compiled
# JavaScript application ready for production.
RUN npm run build

# ------------------------------------------------------------------------------
# PRODUCTION STAGE
#
# This stage creates the final image that will actually run
# in production.
#
# Instead of copying everything from the build stage,
# we create a clean image and only copy what is necessary.
#
# This makes the final image:
# - Smaller
# - Faster to download
# - More secure
# ------------------------------------------------------------------------------

FROM node:18-alpine3.19

WORKDIR /usr/src/app

# Copy package files again because this is a completely new image.
COPY package*.json ./

# Install ONLY production dependencies.
#
# The "--omit=dev" flag skips packages like:
# - TypeScript
# - Nest CLI
# - Jest
# - ESLint
#
# These packages were only necessary during the build stage.
#
# After installation, clean the npm cache to reduce
# the final image size.
RUN npm ci --omit=dev \
  && npm cache clean --force

# Copy only the compiled application from the build stage.
#
# Notice that we are NOT copying:
# - src/
# - tests/
# - TypeScript files
# - node_modules from the builder
#
# Only the compiled "dist/" directory is needed
# to run the application.
COPY --from=build /usr/src/app/dist ./dist

# Documents that this application listens on port 3000.
#
# EXPOSE does NOT publish the port.
# It only informs Docker (and other developers)
# which port the application uses.
#
# To access it from the host machine you still need:
# docker run -p 3000:3000
EXPOSE 3000

# Defines the main process executed when the container starts.
#
# The container will remain alive as long as this process
# is running.
#
# We use "start:prod" because the application has already
# been compiled during the build stage.
#
# Internally this executes:
# node dist/main.js
CMD ["npm", "run", "start:prod"]