# Base 
FROM node:12-alpine AS base 
ENV NODE_ENV=production 
WORKDIR /app 
COPY package*.json ./ 
RUN npm ci && npm cache clean --force 
ENV PATH /app/node_modules/.bin:$PATH 

# Development 
FROM base AS dev 
ENV NODE_ENV=development 
WORKDIR /app 
RUN npm install --only=development 
CMD ["npm", "start"] 

# Build 
FROM base AS build 

# Need to define the env vars for the React app 
# so that the build process will have access 
# to the values to use for production 

ENV REACT_APP_CATALOG_MANAGEMENT_BASE_URL=http://localhost:8081 
ENV REACT_APP_CUSTOMER_SUPPORT_BASE_URL=http://localhost:8082 
WORKDIR /app 
COPY . . 
RUN npm run build 

# Production 
FROM nginx:stable-alpine AS prod 
COPY --from=build /app/build /usr/share/nginx/html 
EXPOSE 80 
CMD ["nginx", "-g", "daemon off;"]
