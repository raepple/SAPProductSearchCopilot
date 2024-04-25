# Hands-on lab for the SAP Product Search Copilot

## Solution architecture
![](images/solarch.png)

Edit `C:\Windows\System32\drivers\etc\hosts`
40.69.209.245 vhcals4hci.dummy.nodomain vhcals4hci

## Exercise 1: Launch the SAP Product Search Copilot

| Step | Description | Screenshot |
| ----------- | ----------- | ----------- |
| 1.1 | Sign in to [Copilot Studio](https://copilotstudio.microsoft.com/) as `user<N>@bestruncorp.onmicrosoft.com` | ![](images/1-1.png) |
| 1.2 | Switch to your user‘s environment `bestruncorp_user<N>` | ![](images/1-2.png) |
| 1.3 | Open the SAP Product Search Copilot | ![](images/1-3.png) |
| 1.4 | Select **Settings > Channels** | ![](images/1-4.png) |
| 1.5 | Click **Demo website** | ![](images/1-5.png) |
| 1.6 | Copy the URL | ![](images/1-6.png) |
| 1.7 | Open the demo website in a private browser window. Click **Login** | ![](images/1-7.png) |
| 1.8 | Login to the Copilot as `user<N>@bestruncorp.onmicrosoft.com` | ![](images/1-8.png) |
| 1.9 | Copy the login validation code | ![](images/1-9.png) |
| 1.10 | Send the code | ![](images/1-10.png) |
| 1.11 | Start the conversation e.g. by typing *„Hi! How do I get started?“* until you run into an error | ![](images/1-11.png) |

## Exercise 2: Troubleshoot the SAP access in Power Automate

| Step | Description | Screenshot |
| ----------- | ----------- | ----------- |
| 2.1 | Open [Power Automate](https://make.powerautomate.com/) in a new browser tab | ![](images/2-1.png) |
| 2.2 | Switch to your user‘s enviornment `bestruncorp_user<N>` | ![](images/2-2.png) |
| 2.3 | Select **Solutions > SAP Product Search Copilot** | ![](images/2-3.png) |
| 2.4 | Select **Cloud Flows > Chat with Azure OpenAI** | ![](images/2-4.png) |
| 2.5 | Select the last failed instance from the run history | ![](images/2-5.png) |
| 2.6 | Browse to the failed action in the instance trace | ![](images/2-6.png) |
| 2.7 | Check the „innerError“ message | ![](images/2-7.png) |

## Exercise 3: Fix the login error in SAP

| Step | Description | Screenshot |
| ----------- | ----------- | ----------- |
| 3.1 | Click **Edit** | ![](images/3-1.png) |
| 3.2 | Scroll down and click on **+ > Add an action** below the action `Parse response message for action` | ![](images/3-2.png) |
| 3.3 | Select **Control > Condition** | ![](images/3-3.png) |
| 3.4 | Click on the **Flash** icon | ![](images/3-4.png) |
| 3.5 | Select **Body action** from the `Parse response message for action` | ![](images/3-5.png) |
| 3.6 | Select **is not equal to** | ![](images/3-6.png) |
| 3.7 | Enter *none* in the value field | ![](images/3-7.png) |
| 3.8 | Click **Collapse** | ![](images/3-8.png) |
| 3.9 | Click on **+ > Add an action** in the *True* path | ![](images/3-9.png) |
| 3.10 | Enter *Run a child* in the search bar and select **Run a Child Flow** from the list | ![](images/3-10.png) |
| 3.11 | Select **Exchange Token** from the list in **Child Flow** field | ![](images/3-11.png) |
| 3.12 | Click on the **Flash** icon in the **EntraIDToken** field | ![](images/3-12.png) |
| 3.13 | Select the **EntraIDToken** data element from the **Run a flow from Copilot** Trigger | ![](images/3-13.png) |
| 3.14 | Rename the action to *Exchange Entra ID Access Token for SAP Access Token* and click **Collapse** | ![](images/3-14.png) |
| 3.15 | Scroll down and select the SAP data access action **Get all products the user has access to from SAP** from the switch **List categories** case | ![](images/3-15.png) |
| 3.16 | Delete the placeholder text *<fix missing token here>* in the **Authorization** Headers field. Type in *`Bearer<Space>`"* and click on the **Flash** icon | ![](images/3-16.png) |
| 3.17 | Scroll down and select the **SAPToken** data element from the **Exchange EntraID Access Token for SAP Access Token** action | ![](images/3-17.png) |
| 3.18 | Click **Collapse** | ![](images/3-18.png) |
| 3.19 | Repeat the steps **3.15** to **3.18** for the two remaining actions **Get all products for a category from SAP** and **Get product details from SAP** | ![](images/3-19.png) |
| 3.20 | Click **Save draft** | ![](images/3-20.png) |
| 3.21 | Wait for the save operation to complete, then click **Publish** | ![](images/3-21.png) |
| 3.22 | In Copilot Studio, go to **Topics > System**, and select the **Fallback** topic | ![](images/3-22.png) |
| 3.23 | Scroll down to the action where the **Chat with Azure OpenAI** flow is called. Click **Select Variable** on the **EntraIDToken** input field | ![](images/3-23.png) |
| 3.24 | Select the **System** tab and select **User.AccessToken** from the list | ![](images/3-24.png) |
| 3.25 | Click **Save** | ![](images/3-25.png) |
| 3.26 | Go to **Publish** and click **Publish** | ![](images/3-26.png) |

## Exercise 4: Test SAP data access with the fixed flow

| Step | Description | Screenshot |
| ----------- | ----------- | ----------- |
| 4.1 | Go back to the demo website and click *Start over* | ![](images/4-1.png) |
| 4.2 | You may need to re-login to the copilot | ![](images/4-2.png) |
| 4.3 | Start the conversation | ![](images/4-3.png) |
| 4.4 | Continue until you get the details of a product you are interested in | ![](images/4-4.png) |

## Exercise 5: Change the user‘s authorizations in SAP

| Step | Description | Screenshot |
| ----------- | ----------- | ----------- |
| 5.1 | Access the SAP system as user `USER<N>ADM` with the [SAP Web GUI](https://vhcals4hci.dummy.nodomain:44300/sap/bc/gui/sap/its/webgui) | ![](images/5-1.png) |
| 5.2 | Enter transaction code `PFCG` (Role Maintenance) and hit **Return** | ![](images/5-2.png) |
| 5.3 | Enter *`PRODUCT_CATALOG_USER<N>`* in the **Role** field and hit the **Edit** button | ![](images/5-3.png) |
| 5.4 | Switch to tab **Authorizations** and click **Change Authorization Data** | ![](images/5-4.png) |
| 5.5 | Expand the authorization object tree and click the **Edit** button for the *PDCATEGORY* authorization field | ![](images/5-5.png) |
| 5.6 | Click on the **value help** button of the **'From'** field | ![](images/5-6.png) |
| 5.7 | Select a different product category, e.g. *Batteries*, and confirm with **OK** | ![](images/5-7.png) |
| 5.8 | Also change the **'To'** field by clicking on the **value help** | ![](images/5-8.png) |
| 5.9 | Change the value, e.g. to *Calendars*, and click **OK** | ![](images/5-9.png) |
| 5.10 | Click **Save** | ![](images/5-10.png) |
| 5.11 | Click **Save** | ![](images/5-11.png) |
| 5.12 | Click **Generate** | ![](images/5-12.png) |
| 5.13 | Click **Exit** | ![](images/5-13.png) |
| 5.14 | Back on the Demo website, click **Start over** | ![](images/5-14.png) |
| 5.15 | Ask the copilot again from which categories you can choose from to see the changes made in the backend | ![](images/5-15.png) |