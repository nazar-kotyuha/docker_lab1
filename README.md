# Dockerizing Note.js app

## Creating Dockerfile

1. FROM node:16
Defining from what image we want to build from. Used the latest LTS version 16 of node available from the Docker Hub.
2. WORKDIR /app
Creating a directory to hold the application code inside the image, this will be the working directory for application.
3. COPY package*.json ./
Installing app dependencies. A wildcard is used to ensure both package.json AND package-lock.json are copied.
4. RUN npm install
This image comes with Node.js and NPM already installed so the next thing we need to do is to install app dependencies using the npm binary.
5. COPY . .
Bundling app's source code inside the Docker image, using the COPY instruction.
6. EXPOSE 80
App binds to port 80 (as given in the task) so using the EXPOSE instruction to have it mapped by the docker daemon.
7. CMD [ "node", "server.js" ]
Defining the command to run app using CMD which defines runtime. Here using 'node server.js' to start server.


## .dockerignore

Creating .dockerignore file to prevent some unwanted files from being copied onto Docker image.



In method service.getUsdToUah() making GET request to https://www.floatrates.com/daily/uah.json, then reading data and
parsing json to created struct service.Result. From parsed result getting USD to UAH currency exchange rate 
(result.Usd.Rate) and returning this from method.

## Getting USD to BTC exchange rate

Code to do this: pkg/service/currency.go

In method service.getUsdToBTC() making GET request to https://blockchain.info/tobtc?currency=USD&value=1, then reading 
data and converting to decimal.Decimal. After that, converted result returning from the method.

## Getting BTC to UAH exchange rate

Code to do this: pkg/service/currency.go

In method service.GetBtcToUah() getting USD to UAH exchange rate from method service.getUsdToUah() and USD to BTC 
exchange rate from service.getUsdToBTC(). After that, calculating BTC to UAH exchange rate and returning result.

## Mailing 

Code to do this: pkg/mail/mail.go

In method checkAddress(string) e-mail address is being validated using method mail.validate() and is being checked if 
this mail was already added to the file (pkg/mail/addresses.txt).
In method mail.SendMail() using smtp.gmail.com e-mails is being sending to addresses stored in pkg/mail/addresses.txt.

## Request 

## /api/rate

GET request with path "/api/rate" used to get current BTC to UAH exchange rate.

![img.png](info/img1.png)

Request example:
curl http://127.0.0.1:8000/api/rate --request "GET"

## /api/subscription/subscribe

POST request with path "/api/subscription/subscribe" used to add (if wasn't added before) e-mail address from formData 
to file pkg/mail/addresses.txt to later send to these addresses e-mails with current BTC to UAH exchange rate.

![img.png](info/img2.png)

Trying to add the same address one more time:

![img.png](info/img4.png)

Request example:
curl http://127.0.0.1:8000/api/subscription/subscribe \
--include \
--header "application/x-www-form-urlencoded" \
--request "POST" \
--data "email=<put e-mail address here>"

## /api/subscription/sendEmails

POST request with path "/api/subscription/subscribe" used to send e-mail with current BTC to UAH exchange rate to 
addresses stored in file pkg/mail/addresses.txt.
To show this request working will add some temporal mail addresses to file using /api/subscription/subscribe POST 
request

![img.png](info/img5.png)

![img.png](info/img6.png)

![img.png](info/img7.png)

![img.png](info/img8.png)

E-mail were sent to all stored addresses.

Request example:
curl http://127.0.0.1:8000/api/subscription/sendEmails --request "POST"

## Docker

Dockerfile added.

Example:
docker build -t golang-api .

![img.png](info/img9.png)

Working in docker:

![img.png](info/img10.png)

