
# Concept


- azure cdn service defines endpoints
- each endpoint could be associated to a backend service
- rules per endpoint
    - single global rule
        - ignore
        - override
        - add when missing
    - custom rule
        - order matters
        - set up on extensions / path
    - query string
        - ignore : same cached asset for all uri 
        - bypass : no cache, hit backend everytime
        - unique : each uri has its own cached asset