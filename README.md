# Order Management and Processing

## Functional Requirements
### 1. Order Management
- Place an order using `POST /orders`
- Add to queue for processing with status `Pending`
### 2. Order Processing
- Asynchronous processing `~2-5 seconds/request`
- Update status to `Processing`
- Simulate and process the order and update status to `Completed` 
### 3. Order Status
- Check order status using `GET /orders/{order_id}/status`
### 4. Metrics
- Get the metrics using `GET /metrics`
  - Total number of orders processed 
  - Average order processing time 
  - Count of orders in each status

## Non-functional requirements
### Scalability
- System should handle `1,000` concurrent orders
### Performance
- Order processing should simulate real-world delay `3 seconds/order fixed`
### Reliability & Fault Tolerance
- If queue is full, system rejects new orders `(429 Too Many Requests)`
### Monitoring
- Custom Metrics API for order processing metrics

## Assumptions
### Order Handling
- Orders cannot be canceled once placed
- Orders always succeed (no payment/inventory failures handled)
- Fixed order processing time `~3 seconds/order`

### Queue
- Single-node system
- Sufficient worker threads for order processing. Calculations in estimation section
- Orders marked as `Processing` for `>10 minutes` are re-queued

### API Design
- Follows REST conventions

## Estimations
```asciidoc
- Order processing time = 3 sec
- Orders that can be processed per second: `1/3 = 0.33 orders/sec`
- Concurrent order requests: `1000` - Queue size
- CPU cores: 8
- Threads/core: 3
- Total threads: CPU cores * Thread/core = 8 * 3 = 24 threads
- Time taken to process orders using above threads: 24 * 0.33 = ~ 8 orders/sec
- Total time to process orders: Total requests / orders processed per second = 1000 / 8 = 125 sec
```