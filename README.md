TCPEvents
==========

[EventGhost](http://eventghost.net) plugin for sending and receiving events and data over a network.
TCPEvents was created as a replacement for the very limited Network Event Sender and Receiver plugins and adds many enhancements. It is fully compatible with the original Network Event Sender/Receiver plugins. This is a fork of the [original TCPEvents](http://www.eventghost.net/forum/viewtopic.php?t=2944) which appears to have been abandoned. It fixes a security vulnerability and adds some useful features.


#### Installation
- Download https://github.com/per1234/TCPEvents/archive/master.zip
- Unzip and move the folder to the EventGhost\plugins folder.
- Restart EventGhost if it is running.
- Add the TCP Events plugin to your EventGhost tree.
- Configure the TCP Events plugin with your network settings.


<a id="usage"></a>
#### Usage
You can use {} in most configuration fields to have EventGhost replace the content with the corresponding variable.
- **Plugin Configuration**:
  - **TCP/IP Port** - The port used for receiving.
  - **Password** - The password must match the password used by the sender. Leave the password field blank to disable authentication. Unauthenticated operation is not supported by the Network Event Sender/Receiver plugin.
  - **Default Event Prefix** - The prefix to use on received events unless a prefix is specified by the sender.
  - **Add source IP to the payload** - If checked the sender's IP address will be included with the payload of received events.
  - **Connection Timeout(seconds)** - Maximum number of seconds to attempt to connect to the server before the event send fails. Any other operation in EventGhost will be blocked until the send is completed or times out so it is important to find the smallest value that still allows for reliable communication.
  - **Communication Timeout(seconds)** - Maximum number of seconds to attempt communication with the server before the event send fails.

- **Send an Event**
  - **Address** - The IP address to send the event to.
  - **TCP/IP port** - The port to send the event to. This should match the port setting in the plugin configuration of the receiver.
  - **Password** - Leave blank to disable authentication.
  - **Prefix** - Prefix of the sent event. If the prefix is not specified then the default prefix specified in the plugin configuration of the receiver will be used. Custom prefix is not supported by the Network Event Sender/Receiver plugin.
  - **Suffix** - Suffix of the sent event.
  - **Payload(Python expr.)** - If you want to send a plain text string write it between quotes. You can send/receive payload of various types(strings, numbers, lists, dicts, tuples, datetime, etc.).

- **Send Data** - When sending data, the server won't produce any event. It will only store it with the provided name. The stored data can be retrieved at any time using the data name. This action is not supported by the Network Event Sender/Receiver plugins. See the **Send an Event section** for documentation of duplicate fields.
  - **Name** - The name is used to retrieve received data.
  - **Data(Python expression)** - Data to send.

- **Retrieve Received Data** - The retrived data stored under the name is returned as eg.result. This action is not supported by the Network Event Sender/Receiver plugins.
  - **Name of the data to retrieve** - Use the data name specified in the Send Data action.

- **Request Data from a remote host** - The response is returned as eg.result. No event is created. This action is not supported by the Network Event Sender/Receiver plugins. See the **Send an Event** section for documentation of duplicate fields.
  - **Python expression** - This expression is evaluated on the receiver and the result is sent back


<a id="authentication"></a>
#### Authentication process
TCPEvents uses MD5 encrypted APOP style authentication to avoid sending passwords in plaintext.
- sender: Connect to receiver. Send `quintessence\n\r`.
- receiver: Send cookie.
- sender: The password is appended to the cookie and the MD5 digest is calculated and sent back to the receiver.
- receiver: If the received MD5 digest is correct then it sends accept.
- sender: Send `payload {payload string}\n{event}\nclose\n` to the receiver.
- receiver: Close the connection to the sender.


#### Acknowledgements
- TCPEvents is based on the Network Event Sender and Receiver plugins by bitmonster, the creator of EventGhost.
- TCPEvents was written by EventGhost forum member miljbee.


<a id="changelog"></a>
#### Changelog
- see http://www.eventghost.net/forum/viewtopic.php?t=2944 for the original changelog
- Security vulnerability patch
- Unauthenticated option
- Set timeouts via configuration
