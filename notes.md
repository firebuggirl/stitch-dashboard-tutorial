# Create a MongoDB Stitch Dashboard

  https://docs.mongodb.com/stitch/tutorials/dashboard/


  - Install stitch-cli

        ` npm install -g mongodb-stitch-cli `

  - Clone Repo examples

        ` git clone https://github.com/mongodb/stitch-examples.git `



  - ` cd stitch-examples/dashboard/appdir/dashboard-stitch `


        - contains these configurations:


            - `An email/password authentication provider`

            - `A MongoDB service` to link w/ cluster

            - A Stitch `function` named `salesTimeline`

            - A Stitch `function` named `getPopularToppings`

            - An `HTTP service` named `PizzaOrderAPI`


        - create a new Stitch application that is populated with the above configurations:


            - Go to correct directory

              ` cd stitch-examples/dashboard/appdir/dashboard-stitch `


           - Authenticate a MongoDB Atlas User

              ` stitch-cli login --username=<MongoDB Cloud username> --api-key=<MongoDB Atlas API Key> `

          - Optional: Update MongoDB Atlas Cluster Name

              - If your Atlas cluster is `not named Cluster0`, edit the value of clusterName in the `dashboard-stitch/services/mongodb-atlas/config.json` file to match your cluster’s name.

                  - NOTE: this order of operations does NOT work when trying to link the proper cluster. Had to link via Stitch Apps UI instead


          - Import the Application Directory


              ` stitch-cli import `//from within the dashboard-stitch directory


        - Set Up the Dashboard Client


            - need to connect the client code with the Stitch application so that the dashboard can query and display data


            - dashboard client code is located in the `stitch-examples/dashboard` directory


                - two files that communicate with Stitch via 2 file:


                    - ` dashboard.js`:

                        -  queries data from MongoDB

                              - `salesTimeline()` + `getPopularToppings()`

                   - `data_generator.js`:


                        - simulates customer orders via `POST` via `addOrder` webhook





## Create an Email/Password User

    - `Stitch UI` -> `Users` - `Add New User`


      - Username	`example@email.com`

      - Password	`mypassword`

      - -> `create`

## Configure the addOrder Webhook to Run as the New User


    - `Stitch UI` -> `Services` - ` PizzaOrderAPI` -> `addOrder` -> `Settings` tab, set `Run Webhook As` to `User Id` then click the `Select User` button that appears.


    - Choose the email/password user that you just created from the list of users then click `Select User` -> `save`


          - note the values of the `Webhook URL` and `Secret` on this page


## Configure the Data Generation Script


    - `data_generator.js` generates new pizza orders via `addOrder` webhook

        - connect script to webhook via `webhook URL` + the webhook `secret` appended as a query parameter


            -  `data_generator.js`, find and replace `<YOUR WEBHOOK>` w/ your Webhook URL  + append the `secret` as parameter

                ` <webhook url>?secret=yummypizza `


        - `cd stitch-examples/dashboar` -> ` npm install `


## Connect the Dashboard Client to your Stitch Application

    - In `stitch-examples/dashboard/dashboard.js` -> replace `<YOUR APP ID>`


            - find your ID via `clients` tab


## Run the Data Generation Script


      - Upload data from `dashboard` directory to cluster


            - ` node data_generator.js `


                    -  should see a continuous stream of documents being output in your shell as they are inserted into cluster ...terminate the script process when tutorial is finished


## Launch the Dashboard


    - From new shell via `dashboard` directory:

        ` npm start `



## Log In to the Dashboard


    - ` localhost:8080 `

       - Log in and view data as it is updated
