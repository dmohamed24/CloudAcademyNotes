When doing back-of-the-envelope calculations for system design, there are some common average metrics and approximations you can use. These are rough estimates that can help you quickly evaluate the feasibility of a design or size components. Here’s a list of typical metrics you might consider:

### Data Sizes

- **Character:** 1 byte (UTF-8 encoding)
- **Integer (int):** 4 bytes (32-bit)
- **Floating point (float):** 4 bytes (32-bit)
- **Double precision (double):** 8 bytes (64-bit)
- **Boolean:** 1 byte (though can be as small as 1 bit in certain contexts)
- **Date/Time:** 8 bytes (64-bit timestamp)
- **Image size:**
    - Thumbnail (small): ~10 KB
    - Medium-sized image: ~100 KB
    - High-resolution image: ~1 MB
- **Video size:**
    - 1-minute 1080p video: ~100 MB (compressed)
    - 1-hour 1080p video: ~6 GB (compressed)

### Network and API

- **Request size:**
    - Simple HTTP request: ~0.5 KB - 2 KB
    - Average API request: ~2 KB - 5 KB
- **Response size:**
    - Small API response (e.g., JSON): ~1 KB - 5 KB
    - Average HTML page: ~20 KB - 100 KB
    - Single API call with media (e.g., image): ~50 KB - 1 MB
- **Network latency:**
    - Within a data center: ~0.5 ms - 1 ms
    - Within the same region (same city): ~10 ms - 50 ms
    - Between different regions (e.g., different continents): ~100 ms - 300 ms

### Storage

- **Text document:**
    - 1 KB per page of plain text (about 2,000 characters)
    - Word document (with formatting): ~20 KB per page
- **Database row size:**
    - Simple record (few fields): ~100 bytes
    - Complex record (many fields, with indexing): ~1 KB - 10 KB
- **Database size estimation:**
    - Number of records * average record size
    - Add ~20-30% overhead for indexing, metadata, etc.
- **Log file entry:**
    - Simple log entry: ~100 bytes - 500 bytes
    - Complex log entry (with stack trace): ~1 KB - 10 KB

### User Activity

- **Pageviews per user per day:** ~10-50
- **API calls per user per day:** ~10-100
- **Average session duration:** ~5-10 minutes
- **Daily active users (DAU):** ~10-30% of total users
- **Monthly active users (MAU):** ~30-60% of total users

### System Throughput and Performance

- **Network bandwidth:**
    - 1 Gbps = ~125 MBps (bytes per second)
    - 10 Gbps = ~1.25 GBps
- **Disk I/O:**
    - SSD: ~500 MBps - 3 GBps (read/write)
    - HDD: ~100 MBps - 200 MBps (read/write)
- **Memory access speed:** ~100 nanoseconds (ns) - 200 ns
- **CPU operations:**
    - Simple arithmetic operation: ~1 ns - 10 ns
    - Cache hit: ~10 ns - 100 ns
    - Disk seek: ~5 ms - 15 ms

### Traffic and Scaling

- **Requests per second (RPS):**
    - Small web application: ~100 - 1,000 RPS
    - Medium web application: ~1,000 - 10,000 RPS
    - Large web application: ~10,000 - 100,000+ RPS
- **User growth:**
    - Monthly growth: ~5% - 20% (depending on the stage of the product)
- **Data growth:**
    - Logs and analytics data: ~1 GB - 10 GB per day (for medium-sized applications)
    - User-generated content (e.g., images, videos): Varies significantly, but plan for exponential growth.

### Cost Estimation

- **Server cost (cloud):**
    - Small instance (1-2 vCPUs, 4-8 GB RAM): ~$0.01 - $0.05 per hour
    - Medium instance (4-8 vCPUs, 16-32 GB RAM): ~$0.10 - $0.20 per hour
    - Large instance (16+ vCPUs, 64+ GB RAM): ~$0.50 - $2.00 per hour
- **Storage cost (cloud):**
    - Block storage (SSD): ~$0.10 - $0.20 per GB per month
    - Object storage (e.g., S3): ~$0.02 - $0.03 per GB per month
- **Data transfer cost (cloud):**
    - Intra-region (same data center): Often free or minimal
    - Inter-region (different data centers): ~$0.01 - $0.10 per GB

### Example Usage

- **User uploads a photo (1 MB) and views it 5 times per day:**
    
    - Storage: 1 MB * 10 million users = 10 TB/day
    - Bandwidth: 1 MB * 5 views * 10 million users = 50 TB/day
- **API handling 10,000 RPS, each request is 2 KB and response 10 KB:**
    
    - Bandwidth: 10,000 * (2 KB + 10 KB) = 120 MBps
- **Database storing user data (500 bytes per user, 1 million users):**
    
    - Storage: 500 bytes * 1 million = ~500 MB (plus indexing overhead)

These metrics are starting points. You can adjust them based on the specific requirements of your application, user base, and technology stack.