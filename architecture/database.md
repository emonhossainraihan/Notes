## Achieving High Availability 

- No single point of failure: any component can crash 
- Reliability: continuous service despite failure
- Fault tolerance: the limitations are well-defined (predicable)
    - connected clients not dropped (maybe notified about limitations)
    - Error page on /login
- Resilience: quick recovery 
    - DB slave promoted to master 