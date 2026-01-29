# CC Lab 2 – Monolithic Architecture Demo

**SRN:** PES1UG23CS320  

This project demonstrates a **monolithic web application** built using FastAPI.  
The lab shows how a monolithic system behaves under failure and how performance
can be improved through internal code optimization.

---

## Setup

### Install Dependencies
```bash
pip install -r requirements.txt
```

### Initialize Database
```bash
python insert_events.py
```

### Run Server
```bash
python -m uvicorn main:app --reload
```

Server runs at:
```
http://localhost:8000
```

---

## Load Testing

Locust was used to test performance of different routes.

```bash
locust -f locust/checkout_locustfile.py
locust -f locust/events_locustfile.py
locust -f locust/myevents_locustfile.py
```

Locust UI:
```
http://localhost:8089
```

Test configuration:
- Users: 1  
- Ramp-up: 1  
- Duration: 30 seconds  

---

## Observed Monolithic Failure

The `/checkout` route initially contained a **division by zero bug**.
Accessing this endpoint caused the **entire application to crash**, demonstrating
a key drawback of monolithic architecture where a single failure impacts all services.

---

## Optimizations Performed

### `/checkout` Route
- **Bottleneck:** Inefficient while-loop used for fee calculation
- **Change:** Replaced loop-based increment with direct summation
- **Result:** Reduced response time and improved efficiency

### `/events` Route
- **Bottleneck:** Large unnecessary loop causing high CPU usage
- **Change:** Removed redundant iterations and simplified data handling
- **Result:** Response time reduced significantly

### `/my-events` Route
- **Bottleneck:** Redundant looping and dummy variable operations
- **Change:** Returned database results directly without extra processing
- **Result:** Faster responses with lower overhead

---

## Conclusion

This lab demonstrates:
- Characteristics of monolithic applications
- Impact of single-point failures
- Use of load testing to identify bottlenecks
- Performance gains achieved through simple code optimizations

---

## Submission Includes
- Public GitHub repository
- Screenshots (SS1–SS9)
- Optimized source code and this README
