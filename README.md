# Final Project Assignment 2: Explore One More! (FP2) 
DUE March 30, 2015 Monday (2015-03-30)
```
#lang racket

(require 
     racket/gui/base
     net/url
     json
     racket/format
)

; --- Query API
(define api-request "https://na.api.pvp.net/api/lol/na/v1.4/summoner/by-name/SUMMONER_NAME?api_key=8864143b-a987-45a8-b49d-53c0a7677100")
(define (query-for-summoner name)
  (define summoner-request (string->url (string-replace api-request "SUMMONER_NAME" name)))
  
  ; Define request parameters, setup for output
  (define sh (get-pure-port summoner-request #:redirections 5))
  (define summoner-hash-str (port->string sh))
  (define summoner-hash-json (string->jsexpr summoner-hash-str))

  
  (define summoner (hash-ref summoner-hash-json name))
  ;I can't figure out how to make it so the "name" in the above line gets evaluted to the input, but read in as a literal 
  ;in order for it to correctly be identified for the hash. as of now, for example if i type in "Dyrus" in the gui box
  ; it says
  ;hash-ref: no value found for key
  ;key: "Dyrus"
  ;yet if i replace name with 'dyrus it works correctly.
  ;If you know of a way to take a variable and have racket read it as a literal, I would love to hear it.
  ;I've tried serching the documentation and googling but I can't seem to find what I'm looking for.
  
  (define summoner-id    (hash-ref summoner 'id))
  (define summoner-name  (hash-ref summoner 'name))
  (define summoner-icon  (hash-ref summoner 'profileIconId))
  (define summoner-level (hash-ref summoner 'summonerLevel))

  (printf "Results for: ~a\n" summoner-name)
  (printf "- ID: ~a\n" summoner-id)
  (printf "- Icon ID: ~a\n" summoner-icon)
  (printf "- Level: ~a\n" summoner-level)
)

; --- Build Frame
(define frame (new frame% 
     [label "League of Legends Statistics Tracker"]
     [width 300]
     [height 300]))

(send frame show #t)

(define msg (new message% 
     [parent frame]
     [label "Welcome to the LoL Stats Tracker."]))

(define sn-input (new text-field% 
     [parent frame]
     [label "Summoner Name: "]
     [init-value "omithegreat"]
     [callback (lambda(f ev)
                 (send f get-value))
                 ]))

(define submit-button (new button% 
     [parent frame]
     [label "Search"]
     [callback (lambda (button event)
                 (let ([v (send sn-input get-value)])
                   (query-for-summoner v))
                 (send msg set-label "Searching for user..."))]))



```

###
I figured out how to use the code markup this time!
I used a GUI library, and expanded on my first exploration to create a popup box where you can search someone's 
in game name and it gets basic stats about them.  Unfortunately, I'm having trouble turning a string into a literal so it accesses the hash correctly;  I'm gonna keep trying at it, I commented the section that's giving me trouble.
