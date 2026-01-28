sequenceDiagram
    participant browser
    participant server

    Note right of browser: User writes a note in the text field and clicks Save button
    
    Note right of browser: JavaScript prevents default form submission and handles the event
    
    Note right of browser: JavaScript adds the new note to the notes array and re-renders the list
    
    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note_spa
    activate server
    Note left of server: Server saves the new note
    server-->>browser: {"message": "note created"}
    deactivate server
    
    Note right of browser: The browser stays on the same page, no reload needed. The new note is already visible.
