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
  (define name-stripped (string-replace name " " "%20"))
  (define summoner-request (string->url (string-replace api-request "SUMMONER_NAME" name-stripped)))
  
  ; (printf name-stripped)
  
  ; Define request parameters, setup for output
  (define sh (get-pure-port summoner-request #:redirections 5))
  (define summoner-hash-str (port->string sh))
  (define summoner-hash-json (string->jsexpr summoner-hash-str))
  (define summoner (hash-ref summoner-hash-json name-stripped))
  
  (define summoner-id    (hash-ref summoner 'id))
  (define summoner-name  (hash-ref summoner 'name))
  (define summoner-icon  (hash-ref summoner 'profileIconId))
  (define summoner-level (hash-ref summoner 'summonerLevel))
  
  (printf "Results for: ~a\n" name)
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

### My Library: (GUI Library)
For my second exploration I chose to utilize the GUI library. On top of that, I decided to implement my previous exploration into my new exploration, thus I built a GUI interface, to interact with my JSON API parser! Pretty cool if you ask me. 
