Plan to develop this later in 2025. Have other projects such as HomeVPN, Quart APIs + AWS, developing mobile applications, and learning hardware through Pi + Arduino before committing serious time and funds into developing this web browser.

# Artemis

A mobile application that uses SMS traffic to allow users to browse the web.

## Mission Statement

Our mission is to make browsing the internet affordable, accessible, and anonymous. This will be accomplished by allowing mobile applications to browse the web with no data via SMS. This will make the interent affordable by allowing end users to browse the internet while only paying for an unlimited text data plan and needing to pay for significantly less or no data depending on their needs. There are also signifiant portions of the globe which do not have access to high speed data but can still send and receive SMS through the cellular network. Other times the data network can become severely congested in large venues such as sporting events and concerts. In these scenarios having the ability to browse the internet without using data will greatly increase the accessibility. Lastly all web requests will be routed through a central server and thus all traffic will appear to come from our servers. We also have direct control over all of our hardware and will only track your personal phone number for enough time to complete your request.

## Design Roadmap

1. Stage One - PoC 
Begin by designing a proof of concept Android application which will use SMS to send requests to a central server, which will be a raspberry pi with Sixfab modem and SIM set-up. The central server will be in an area connected to a high speed WiFi network where it will complete the HTTP Request to return the desired response. The central server will use a PiHole DNS such that our web browser will come by default with ad block. Once the HTTP Request has been completed we will compress it with Google's new Brotli specially designed for HTML compression. We will then encrypt it with AES-256 and send the compressed and encrypted web response back to the mobile application where it will be decrypted and uncompressed before being displayed in the app as raw HTML.

2. Stage Two - Establishing the Product / Gaining Trust
Once we have a functioning SMS based test browser we will perform throughput testing to get a base benchmark for one server one application network speeds. One SMS message contains 1,120 bits or roughly 1kb of information. The standard rule is that one device can send 10 SMS messages a minute, however by taking advantage of existing technologies such as A2P 10DLC numbers other services such as Twilio has been able to send up to 100 SMS messages per second. Once the product is established we can apply for similar privileges through cellular providers. By sending 100 messages per second and throttling our own servers to match this speed (to limit packet drops / blocking by providers) we could optimally get speeds up to 100kbps for a single server.

3. Stage Three - Scalability / Protocol for Maximizing Throughput
Once we have one server producing an information throughput of roughly 100kbps we will then expand to a master slave model on the server side. Each mobile application will still send web requests to the master server via SMS and that message will be received and processed by the master server. The server will then split the response up into N separate chunks, assign an ID to each, and then compress and encrypt each in parallel. The slave servers will be connected to the master server either by ethernet or a high speed LAN connection such that communication between master and slave is extremely fast. From here the slaves will then send SMS messages to the mobile application where the application will then decrypt, decompress, and restructure the packets to form the complete response very similar to TCP. Once this is done we will test master-slave server set-up with 2, 5, 10, 25, and 50 slaves to get benchmark speeds on how greatly extra slaves increase our total throughput. While it may not be exactly N times as fast due to extra processing times with the TCP-like protocols it should increase it greatly.

4. Stage Four - Adding Accessories
Each server will have a modem with access to cellular and GPS data. We can then expose this GPS data to the end user such that they can select a master server to communicate with that is both nearby and such that they will not be charged extra fees due to international communication. We hope in the end to provide a browser that will come with ad-block and allow for remote browsing of the interent with reasonable data speeds. Depending on overall throughput benchmarking numbers our goal will be to either stream video through the browser or just quickly display complete web pages including images and text to the end user.

## Hardware + Costs
Each server would consist of a raspberry pi, USB wifi dongle, SixFab modem hat, modem, antennas, and SIM. If bought in bulk can be brought down to $150 per server. Each SIM on the server would be somewhere between $5-$20 a month depending on throughput benchmarking.

Android App would cost a $25 one time fee to produce the applicaiton to the Google Play Store.

## Extra Research
* MMS uses data but could greatly increase our throughput. This would still achieve our mission statement but may make it less accessible due to needing some data to receive. However most data plans include MMS messages in their unlimited texting plan so may still allow users to save greatly on their cellular plan. `Must be transparent to users about what plans allow this.`

* As of right now iOS does not allow third party application to send and receive messages. While this has previously been allowed with iOS shortcuts as of iOS 18 these have been removed. Going forward it would be great if these shortcuts were added back or it became possible to do this as we could then deploy an iOS app which would cost $99 annually.
