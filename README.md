<center><img src="https://jfqcza.bn1.livefilestore.com/y2paqP_3uagi8J3WlP4-pNt4kJoOzRKmuohSQUsrjaegIaoNbNZJ7EXLEflIO6XYAOKM6scpKxbtXPg10RL5OO3A9bc6m-zERVRHUYB1OEGq8s/Concur_Logo_VT_Color_500px.png?psid=1" width="190px" /></center>

What is Concur?
--

Concur is a travel and expense software solution that lets 25M business travelers book their travel, capture expense, submit expense reports, and more.  Concur processes over 55 million transactions every year, representing $50 billion spend on T&E.

<center>
<img src="https://jfqcza.bn1.livefilestore.com/y2prf7yXWtqtcsjpFh2XXu6XQ7ppeIg8SqyBAFEUIcqwGG-TQz1vpZhxx6ac2ySbguL5EAsg0IerEzlUQRynANwyVmyt9dUxWWwNS-x75NX248/Screen%20Shot%202014-05-31%20at%2012.02.06%20PM.png?psid=1" /></center>

What can I do with Concur APIs?
--

The **Travel/Itinerary API (v1.1)** lets you access a Concur user's itinerary, including hotel/flight booking info.  As an example, you can mash-up the Itinerary API with a restaurant database/API to provide recommendations of places to eat near a COncur user's hotel booking.

The **Expense API (v3.0)** allows you to get (and push) a Concur user's expense information, including expense line items, their types (e.g. food, lodging), totals, and even receipt images.

Here's an example on how to get an Itinerary List of a Concur user:

    curl https://www.concursolutions.com/api/travel/trip/v1.1
    -H "Authorization: OAuth <insert your access token here>"
    
To get an expense summary by using the Expense Report Digest API:

    curl https://www.concursolutions.com/api/v3.0/expense/reportdigests
    -H "Authorization: OAuth <insert your access token here>"
    -H "Accept: application/json"
    
**To get an access token** (or learn how to generate one), please click [here](#token) to advance to the section below.

You can interact with the API using Swagger [here](https://www.concursolutions.com/api/docs/index.html)

<a href="https://www.concursolutions.com/api/docs/index.html"><img src="https://jfqcza.bn1.livefilestore.com/y2phbDjCiRKZp3Btneho4UncjWK46iv8GFhyKdTZ7xJJ9BCJ-JJiNrw7ieCenHdSGPgEZL0Tj7IYjGypUv61u3KdIrjj0dAUM35DjC3cPzLls0/Screen%20Shot%202014-05-31%20at%202.09.43%20PM.png?psid=1" width="610px" /></a>

For the Itinerary APIs (v1.1), full API documentation, and apps that use the APIs, please refer to:

[https://developer.concur.com/get-started/webservices-overview](https://developer.concur.com/get-started/webservices-overview)

Ideas for a Concur-powered business travel app:
--


Mash-up  | Idea
------------- | -------------
**Concur + Uber** | Send Uber ride expense directly to Concur using Quick Expense or E-Receipt API
**Concur + weather** | Predict weather using Concur traveler's destination and date
**Concur + smartwatch/ring**  | Use smartwatch/ring to confirm if Mastercard purchase line item should be sent to Concur as expense (or ignored if personal purchase); Display QR code to POS reader to send purchase as expense to Concur
**Concur + beacons/indoor position**  | Use Concur user's expense history to push relevant proximity ads/recommendations; Determine walk-time to gate based on Itinerary info
**Concur + credit/debit card**  | Use smartwatch/ring to confirm if Mastercard purchase line item should be sent to Concur as expense (or ignored if personal purchase)
**Concur + parking** | User Concur itinerary info (hotel/flight booking) to find fastest route to parking
**Concur + wifi service** | Identify if colleagues are on same flight and initiate chat session

Find more app ideas [here](https://github.com/ismaelc/ConcurSharingEconomyHack#example)

Incentives
--

- **5 free Devcon passes worth $99 each** to 5 teams (1 pass per team) for the **"Best Use of Concur APIs"** (entries have to be uploaded as short YouTube videos ~1min or less)
- Any team that uses the Concur API has a shot at two $50K prizes when they pitch their app to [The Perfect Trup Fund Awards](https://developer.concur.com/devcon/PerfectTripFundAwards)
- Winners will be promoted to Concur's Blog http://concur.com/blog


<a name="addl">Additional Information</a>
--
The subsections below provide a more detailed information on how to:

  - [Generate an access token](#token)
  - [POST/GET dummy data to/from Concur](#dummy)
  - [Sample code](#samplecode)
  - [Example apps in the App Center and previous hackathons](#example)
  - **Stuck?**  Contact Chris!
    - *chris.ismael@concur.com*
    - *@ConcurAPI* or *@chrispogeek*


 <a name="token">**Generate an access token**</a>
 --

  1. **Get your Consumer Key**  

 After logging in to http://concursolutions.com, go to Administration -> Register Partner Application -> Concur Partner Application (Modify).  We need the consumer key so we can call the endpoint that would return the access token. (**Note: Get your Concur login/password from Chris at the Concur booth or email him at chris.ismael@concur.com**)

  <img src='http://chrispogeek.files.wordpress.com/2014/01/untitled.png' width="600px" />

  2.  **Call the access token request endpoint**

  Here's what the HTTP call looks like to request for an access token:

        GET https://www.concursolutions.com/net2/oauth2/accesstoken.ashx HTTP 1.1
        Authorization: Basic am9obl9kZXZlbG90bWFpbC5jb206VHJhdmVsJkV4cGVuc2UkMjAxMg==
        X-ConsumerKey: eZByXv2X41cJlC21pSVvRi    

  Note that we already have `X-ConsumerKey` from number 1.  To generate the `Authorization: Basic` value, you need to Base64-encode the login ID, colon and password such that john_developer@hotmail.com:Travel&Expense$2012 becomes am9obl9kZXZlbG90bWFpbC5jb206VHJhdmVsJkV4cGVuc2UkMjAxMg==.  If you're not doing this programmatically yet, you can use this [nifty web tool](http://www.base64encode.org/) to generate that value (remember to choose Encode).

  If you're using a Terminal, an equivalent curl statement of the above would be:
        
        curl https://www.concursolutions.com/net2/oauth2/accesstoken.ashx
        -H "Authorization: Basic am9obl9kZXZlbG90bWFpbC5jb206VHJhdmVsJkV4cGVuc2UkMjAxMg=="
        -H "X-ConsumerKey: eZByXv2X41cJlC21pSVvRi"

  If the call is successful, you should get an XML response with a `<Token>` node.  That's your access token. We recommend that you use [Postman](http://www.getpostman.com/), a Chrome extension, to help you manage your API calls (not just Concur ones).  Here's what the API calls look like in Postman:

  <img src='http://chrispogeek.files.wordpress.com/2014/01/screen-shot-2014-01-13-at-4-45-35-pm.png' width="600px" />

  3.  **Call the APIs to pull expense report items**

 After getting an access token, we can pull dummy data from the Expense Report Digest, like so (in Terminal):

        curl https://www.concursolutions.com/api/v3.0/expense/reportdigests
        -H "Authorization: OAuth <insert your access token here>"
 
  This would return an Expense Report response, with a field called `ID`.  We need this ID to extract the expense line items we created in the app earlier.  To liven things up a bit, let's use the [Swagger](https://www.concursolutions.com/api/docs/index.html#!/Entries) documentation of the "Entries" API to get the individual expense line items:

  <img src='https://jfqcza.bn1302.livefilestore.com/y2pExFhXefF7BWcggCWbeRkUyOUyEf1UdRi0HoCcpj8PKfaGMub-K8xc0BXHsX2NUFlNp54-m6X3aJ7dRNLQiIHfyYJhdtTiEJEArnZJ-r7NCA/Screen%20Shot%202014-04-22%20at%201.21.55%20PM.png?psid=1' width="600px" />

  We highlighted two things here, the (oval) field where you put in your access token, and the (rectangle) field where you put in the `ID` we got from the previous API call.  Note that we can do this same call in curl, or in any fashion you want.  Swagger just provides us a consolidated way to make the API calls.

  To execute the call, click the "Try it out!" button.  You should get a response like this below:

  <img src='https://jfqcza.bn1304.livefilestore.com/y2pty6lMBv5XXLjA_mT5HLpSNea4hVr3AUCRuEI207Wr1otLVxy86klHYuNDP0N-cvb75IFJvicR1jR2K7X3wqJXsH_AQNcEWkp6iO4t3jXRCs/Screen%20Shot%202014-04-22%20at%201.29.22%20PM.png?psid=1' width="600px" />

 **Note: The steps demonstrated above follow the OAuth Native Flow.  If you prefer something similar to a Facebook login experience, you can check out the OAuth Web Flow [here](https://developer.concur.com/api-documentation/oauth-20-0)**

[Back to Top](#addl)

<a name="dummy">**POST/GET dummy data to/from Concur - the API or app**</a>
--

  1. **Pushing dummy data to Concur using the API**  

    POST a dummy Itinerary to Concur:

          Request Type = POST
          Authorization = OAuth <your access token>
          URI = https://www.concursolutions.com/api/travel/trip/v1.1/
          ContentType = Application/XML
          Body = 
           <?xml version="1.0"?>
           <Itinerary xmlns="http://www.concursolutions.com/api/travel/trip/2010/06">
           <TripName>TechCrunch Disrupt Concur</TripName>
           <StartDateLocal>2014-05-30T03:47:14</StartDateLocal>
           <EndDateLocal>2014-06-06T03:47:14</EndDateLocal>
           <Bookings>
            <Booking>
             <Segments>
                 <Hotel>
                     <Status>HK</Status>
                     <StartCityCode>SFO</StartCityCode>
                     <StartDateLocal>2014-05-30T07:47:14</StartDateLocal>
                     <EndDateLocal>2014-06-06T03:47:14</EndDateLocal>
                     <Name>Times Square Hilton New York</Name>
                     <RecordLocator>Hotel Locator</RecordLocator>
                     <RoomDescription>1 KING BED ACCESSIBLE ROOM - K1RRC</RoomDescription>
                     <Currency>USD</Currency>
                     <CancellationPolicy>Cxl 1 day prior to Arrival</CancellationPolicy>
                     <DailyRate>240.3500</DailyRate>
                     <NumRooms>1</NumRooms>
                     <NumPersons>1</NumPersons>
                     <RateCode>LV4</RateCode>
                     <Charges>
                         <Rate>
                             <Currency>USD</Currency>
                             <Amount>10.00</Amount>
                             <StartDatelocal>2014-05-30T07:47:14</StartDatelocal>
                             <IsPrimary>false</IsPrimary>
                             <SemanticsCode>ROOMRATE</SemanticsCode>
                             <PerUnit>DAY</PerUnit>
                             <NumUnits>3.00</NumUnits>
                         </Rate>
                     </Charges>
                 </Hotel>
             </Segments>
             <RecordLocator>Disrupt123</RecordLocator>
             <BookingSource>Sample Itin for Disrupt</BookingSource>
             <DateBookedLocal>2014-04-30T03:47:14</DateBookedLocal>
           </Booking>
           <Booking>
             <Segments>
                 <Air>
                     <Vendor>AA</Vendor>
                     <FlightNumber>425</FlightNumber>
                     <StartCityCode>SFO</StartCityCode>
                     <StartDateLocal>2014-05-30T03:47:14</StartDateLocal>
                     <EndCityCode>NYC</EndCityCode>
                     <EndDateLocal>2014-05-30T07:47:14</EndDateLocal>
                     <Cabin>O</Cabin>
                     <ClassOfService>O</ClassOfService>
                 </Air>
             </Segments>
             <RecordLocator>Air Locator</RecordLocator>
             <BookingSource>Sample Itin for Disrupt</BookingSource>
             <DateBookedLocal>2014-04-30T03:47:14</DateBookedLocal>
           </Booking>
         </Bookings>
         </Itinerary>
   
    **Hack tip :** More details on the Itinerary API v1.1. [here](https://developer.concur.com/api-documentation/web-services/itinerary/itinerary-resource/itinerary-resource-get)
         
    POST Expense Report Header to Concur: 

         Request Type = POST
         Authorization = OAuth <your access token>
         URI = https://www.concursolutions.com/api/expense/expensereport/v1.1/report
         ContentType = Application/XML
         Body = 
         <Report xmlns="http://www.concursolutions.com/api/expense/expensereport/2011/03">
             <Name>Disrupt Hackathon NYC Trip</Name>
             <Purpose>All Hackathon Expenses</Purpose>
             <Comment>Includes hotel and meals.</Comment>
             <OrgUnit1>US</OrgUnit1>
             <OrgUnit3>Bellevue</OrgUnit3>
             <Custom1>Client</Custom1>
             <Custom2>Local</Custom2>
             <UserDefinedDate>2014-05-01 15:15:07.0</UserDefinedDate>
         </Report>
    **Hack tip 1:** You can POST using JSON by setting the header to `Content-Type:application/json` and checking out a sample JSON body in our [Swagger](https://www.concursolutions.com/api/docs/index.html#!/Entries) doc.  Note that the examples here use v1.1, and those shown in Swagger use v3.0.

    **Hack tip 2**: You need to generate a report header to be able to submit an expense entry.  A collection of entries is a report.
        
    *Click [here](https://developer.concur.com/api-documentation/web-services/expense-report) to view the relationship of an Expense Report Header to an Expense Report Entry and search for **Expense Report Processes**.   *
    
    GET Expense Report Details:

         Request Type = GET
         Authorization = OAuth <your access token>
         URI = https://www.concursolutions.com/api/expense/expensereport/v2.0/Reports/?reportcountry=US
         ContentType = Application/JSON

    POSTing Expense Report Entry:

         Request Type = POST
         Authorization = OAuth <your access token>
         URI = https://www.concursolutions.com/api/expense/expensereport/v1.1/report/B6F4FD62FB424911A3B8/entry
         ContentType = Application/XML
         Body = 
         <ReportEntries xmlns="http://www.concursolutions.com/api/expense/expensereport/2011/03">
         <Expense>
             <CrnCode>USD</CrnCode>
             <ExpKey>BRKFT</ExpKey>
             <Description>Starbucks for Breakfast</Description>
             <TransactionDate>2014-05-01</TransactionDate>
             <TransactionAmount>15.54</TransactionAmount>
             <Comment>Breakfast meeting</Comment>
             <VendorDescription>Starbucks</VendorDescription>
             <IsPersonal>N</IsPersonal>
         </Expense>
         </ReportEntries> 
  2. **Pushing dummy expense data to Concur using the app**
   
   You can access the web version of Concur at http://concursolutions.com/ or get the mobile app from [App Store](https://itunes.apple.com/us/app/concur-travel-receipts-expense/id335023774?mt=8) or [Google Play](https://play.google.com/store/apps/details?id=com.concur.breeze).

   <center>
<img src="https://jfqcza.bn1.livefilestore.com/y2prf7yXWtqtcsjpFh2XXu6XQ7ppeIg8SqyBAFEUIcqwGG-TQz1vpZhxx6ac2ySbguL5EAsg0IerEzlUQRynANwyVmyt9dUxWWwNS-x75NX248/Screen%20Shot%202014-05-31%20at%2012.02.06%20PM.png?psid=1" /></center>

   You can view/add new expenses (and even add a receipt image!) to have a variety of data to pull for your API calls. After adding expenses, you can create a report to associate it with by tapping the "Add to Report" button (in the last screenshot above).

[Back to Top](#addl)

<a name="samplecode">Sample code</a>
--------------
- [Windows 8 C#/XAML](https://github.com/ismaelc/Concur-Windows8-SampleCode)
- [Objective-C/iOS](https://github.com/concurtech/mobile-samples)
- [C#](https://github.com/concurtech/api-samples)
- [C#](https://gist.github.com/dberke711/c8d0c9e04c7e76220d4e) (Generate access token native flow)
- [PHP/curl](http://runnable.com/UtWlKVi9ZnsnAABx/upload-receipts-to-concur-using-php-curl) (POST image receipts)
- [Python](https://gist.github.com/Trudeaucj/09c25e79c332e93703a0) (Generate access token)
- [node.js](https://github.com/sckuo/node-concur)
- Java using [Maven](https://github.com/Trudeaucj/Concur-java-examples) or [Gradle](https://github.com/Trudeaucj/Concur-java-examples-gradle)

<a name="example">Example apps</a>
--
All app integrations with Concur can be found in the [App Center](https://www.concur.com/en-us/app-center).  Below are just two examples that would hopefully give you an idea on what to build:

 - [TravelText](https://www.concur.com/en-us/partners/transaction-capture/traveltext) - TravelText allows you to text your expenses right into Concur. No more paper receipts and Excel sheet mayhem!  Check out their video demo [here](https://www.youtube.com/watch?v=sxY_PO-QKZ0).

 - [Trover](https://www.concur.com/en-us/app-center/listing/nDhf34TiiC9RCocFM2xViin5c/Trover) - it's travel photography + business travel integration in one great app. Quoting from this [article](http://skift.com/2013/07/25/travel-photography-app-trover-secures-2-5-million-in-funding-from-concur/#/0), what they get for integrating with Concur is *"being able to tap into Concur’s experience, knowledge and “great visibility into travel patterns”"*.

[Challengepost](http://challengepost.com/software/built-with/concur) also lists Concur API integrations from previous hackathons.  Definitely worth checking out!

[Back to Top](#addl)