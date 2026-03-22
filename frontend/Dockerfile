FROM node:alpine
WORKDIR /frontend
COPY ./package*.json ./
RUN npm install
RUN npm install axios
RUN npm install react-razorpay --save
RUN npm install react-icons --save
RUN npm install react-router-dom --save
RUN npm install --save-dev jest
COPY . .
# Run tests & Then start
CMD [ "npm", "run", "test_start" ]
