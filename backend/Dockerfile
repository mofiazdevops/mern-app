FROM node:21
WORKDIR /app
COPY package.json
RUN npm i
COPY . .
EXPOSE 5050
CMD ["npm", "start"]
