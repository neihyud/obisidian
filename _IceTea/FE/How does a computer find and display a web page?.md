
1. User Input
- The process starts when a user types a URL (Uniform Resource Locator) into the web browser's address bar or clicks on a link.

2. DNS Resolution
- The browser needs to convert the URL into an IP address. It does this using the Domain Name System (DNS). The browser checks its cache for the IP address; if it’s not found, it sends a request to a DNS server.
- The DNS server returns the corresponding IP address for the domain.

3. Establishing a Connection
- Once the IP address is obtained, the browser establishes a connection to the web server using the HTTP (Hypertext Transfer Protocol) or HTTPS (HTTP Secure) protocol. This often involves a TCP/IP handshake to ensure a reliable connection.

4. Sending an HTTP Request
- The browser sends an HTTP request to the server, asking for the specific web page. This request includes information like the type of request (GET, POST, etc.) and any cookies or authentication tokens.

5. Server Response
- The web server processes the request and sends back an HTTP response. This response typically includes:
- Status Code: Indicates the result of the request (e.g., 200 for success, 404 for not found).
- Headers: Additional information about the response (e.g., content type, caching policies).
- Body: The actual content of the web page, usually in HTML format.

6. Rendering the Web Page
- The browser receives the response and begins to render the web page. This involves:
- Parsing the HTML to build the Document Object Model (DOM).
- Fetching and applying CSS (Cascading Style Sheets) for styling.
- Executing any JavaScript code for dynamic content.
- Making additional requests for resources such as images, fonts, and other assets linked in the HTML.

7. Displaying the Page
- After processing all the resources, the browser displays the web page to the user. The rendering process may continue as more resources are loaded and scripts are executed.

8. User Interaction

- The user can now interact with the page, which may trigger further requests and responses as needed (e.g., submitting a form, clicking links).

## How web page get render on the Browser
Rendering process:
	- Parsing HTML -> DOM
	- Parsing CSS -> CSSOM
	- Constructing the Rendering Tree -> combine 2 to create rendering tree
	- Layout -> calulate position and size to rendering
	- Painting
	- Compositing
