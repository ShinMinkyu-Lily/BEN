//(JIRA): Sample Bug Report
  Description
    The link to the registration page on the main landing page is incorrectly configured with the "h4p://" protocol instead of "http://", causing all attempts to access the page to fail.
  Environment
  Browser: Chrome 125.0.6422.142
  OS: macOS Sonoma 14.5
  URL (Landing Page): http://18.216.201.86/
  Steps to Reproduce
    Open a web browser and navigate to the landing page at http://18.216.201.86/.
    Click the "Register" link/button.
    Observe that the browser attempts to navigate to h4p://18.216.201.86/ and fails.
  Expected Result
    The registration page should load successfully at http://18.216.201.86/register.
  Actual Result
    The browser displays a "This site can’t be reached" or "ERR_NAME_NOT_RESOLVED" error, and the page does not load.
  Priority: Highest
  Severity: Blocker
  Labels: registration, bug, frontend, production
  Assignee: benic.kim (or frontend-team)
  Attachments: screenshot_invalid_protocol.png


//(Postman): Bookstore CRUD Collection
{
  "info": {
    "name": "Bookstore CRUD API",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "item": [
    {
      "name": "Create Book",
      "request": {
        "method": "POST",
        "header": [
          { "key": "Content-Type", "value": "application/json" }
        ],
        "body": {
          "mode": "raw",
          "raw": "{ \"title\": \"1984\", \"author\": \"Orwell\", \"isbn\": \"1234567890\" }"
        },
        "url": { "raw": "{{baseUrl}}/books", "host": ["{{baseUrl}}"], "path": ["books"] }
      },
      "event": [
        {
          "listen": "test",
          "script": {
            "exec": [
              "pm.test(\"Status code is 201\", () => pm.response.to.have.status(201));",
              "const jsonData = pm.response.json();",
              "pm.test(\"Response has book ID\", () => pm.expect(jsonData).to.have.property('id'));",
              "// Save bookId to environment for later requests",
              "pm.environment.set('bookId', jsonData.id);"
            ]
          }
        }
      ]
    },
    {
      "name": "Get Book",
      "request": {
        "method": "GET",
        "url": { "raw": "{{baseUrl}}/books/{{bookId}}", "host": ["{{baseUrl}}"], "path": ["books","{{bookId}}"] }
      },
      "event": [
        {
          "listen": "test",
          "script": {
            "exec": [
              "pm.test(\"Status code is 200\", () => pm.response.to.have.status(200));",
              "const jsonData = pm.response.json();",
              "pm.test(\"Returned book ID matches\", () => pm.expect(jsonData.id.toString()).to.eql(pm.environment.get('bookId')));",
              "pm.test(\"Book has title, author, and isbn\", () => {",
              "  pm.expect(jsonData).to.have.property('title');",
              "  pm.expect(jsonData).to.have.property('author');",
              "  pm.expect(jsonData).to.have.property('isbn');",
              "});"
            ]
          }
        }
      ]
    },
    {
      "name": "Update Book",
      "request": {
        "method": "PUT",
        "header": [{ "key": "Content-Type", "value": "application/json" }],
        "body": {
          "mode": "raw",
          "raw": "{ \"title\": \"Animal Farm\", \"author\": \"Orwell\" }"
        },
        "url": { "raw": "{{baseUrl}}/books/{{bookId}}", "host": ["{{baseUrl}}"], "path": ["books","{{bookId}}"] }
      },
      "event": [
        {
          "listen": "test",
          "script": {
            "exec": [
              "pm.test(\"Status code is 200\", () => pm.response.to.have.status(200));",
              "const jsonData = pm.response.json();",
              "pm.test(\"Title updated\", () => pm.expect(jsonData.title).to.eql('Animal Farm'));"
            ]
          }
        }
      ]
    },
    {
      "name": "Delete Book",
      "request": {
        "method": "DELETE",
        "url": { "raw": "{{baseUrl}}/books/{{bookId}}", "host": ["{{baseUrl}}"], "path": ["books","{{bookId}}"] }
      },
      "event": [
        {
          "listen": "test",
          "script": {
            "exec": [
              "pm.test(\"Status code is 204\", () => pm.response.to.have.status(204));"
            ]
          }
        }
      ]
    }
  ]
}
