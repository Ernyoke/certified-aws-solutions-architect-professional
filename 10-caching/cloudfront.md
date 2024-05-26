# CloudFront

- It is a content deliver network (CDN)
- Its job is to improve the delivery of content from its original location to the viewers of the content
- It is accomplishing this by caching and by using an efficient global network

## CloudFront Terms and Architecture

- **Origin**: the source location of the content, can be S3 or custom origin (publicly routable IPv4 address)
- **Distribution**: unit of configuration within CloudFront, which gets deployed out to the CloudFront network. Almost everything is configured within the distribution directly or indirectly
- **Edge Location**: pieces of global infrastructure where the content is cached. They are smaller than AWS regions, but they are way bigger in number and more widely distributed. Can be used to distribute static data only
- **Regional Edge Cache**: larger version of an edge location, but there are fewer of them. Provides another layer of caching
- CloudFront Architecture:
    ![CloudFront Architecture](images/CloudFrontArchitecture1.png)
- If we are using S3 origins, the region edge location is not used in case there is a cache miss for the edge location. Only custom origin can use the regional edge cache!
- **Origin fetch**: the content is fetched from the origin in case of a cache miss on the edge location
- **Behavior**: it is configuration within a distribution. Origins are directly linked to behaviors, behaviors are linked to distributions
    ![CloudFront Behavior](images/CloudFrontArchitecture2.png)
## CloudFront Behaviors

- Distributions are units of configuration in CF, lots of high level options are configured on the distribution level:
    - Price class
    - Web Application Firewall attachment
    - Alternate domain names
    - Type of SSL certificate
    - SNI configuration
    - Security policy
    - Supported HTTP versions
    - etc.
- A single distribution can have one (default behavior) or multiple behaviors
- Any incoming request is pattern matched against behavior's pattern
- Once a request is pattern matched against a behavior, it will become subject ot the behavior's configurations which can be the following:
    - Origin or origin group
    - Viewer protocol policy (redirect HTTP to HTTPS)
    - Allowed HTTP methods
    - Field level encryption
    - Cache directives
    - TTL (min, max, default)
    - Restrict viewer access to a behavior (Trusted Signers)
    - Compress objects automatically
    - Associate Lambda@Edge function

## TTL and Invalidations

![TTL and Invalidations](images/CloudFrontTTL.png)
- And edge location views an object as not expired when it is within its TTL period
- More frequent cache hits = lower origin loads
- Default validity period of an object (TTL) is 24 hours. This is defined in the behavior
- Minimum TTL, maximum TTL: set lower or upper values which an individual object's TTL can have
- Object specific TTL values can be set by the origins using different headers:
    - Cache-Control `max-age` (seconds): TTL value in seconds for an object
    - Cache-Control `s-maxage` (seconds): same as `max-age`
    - Expires (Date and Time): expiration date and time
- For all of these headers if they specify a value outside of minimum, maximum range, the min/max value will be used
- Custom headers for S3 origins can be configured in object's metadata
- Cache invalidations are performed in a distribution and it applies to all edge locations (it takes time)
- Cache invalidation invalidates every object regardless of the TTL value, based on the invalidation pattern
- There is a cost allocated when invalidation is applied
- Instead of invalidation we may consider **versioned file names**
- Versioned file names also help to:
    - Avoid using local browser cache in case of a newer file
    - Help improve logging
    - Reduce cost, no need for manual invalidation
- S3 object versioning and versioned file names should not be confused!

## CloudFront and SSL

- Each CFN distribution receives a default domain name (CNAME)
- HTTPS can be enabled by default for this address
- CF allows alternate domain names (CNAME)
- Process of adding alternate domain names:
    - If we use HTTP, we need a certificate attached to the distribution which matches the alternate name
    - Even if we don't want to use HTTPS, we need a way verifying that we onw and control the domain. This is accomplished by adding an SSL certificate which matches the alternate domain name
    - The result is we need to add an SSL certificate wether we are using or not HTTPS
- SSL certificates are imported using ACM (AWS Certificate Manager). ACM is a regional service, because of this the certificate for global services (such as CF) needs to be imported in *us-east-1* region
- Option we can set on a CFN behavior for handling HTTP and HTTPS:
    - We can allow both HTTP and HTTPS on a distribution
    - We can redirect HTTP to HTTPS
    - We can restrict to only allow HTTPS (any HTTP will fail)
- There are two sets of connections when using CFN:
    - Viewer => CFN (viewer protocol)
    - CFN => Origin (origin protocol)
- Both connections need valid public certificates (self-signed certificates will not work)

## CloudFront and SNI

- Historically every SSL enabled site needed its own IP
- Encryption for HTTP/HTTPS happens on the TCP connection level
- Host header happens after that at Layer 7. It allows to specify to which application we want to connect in case multiple applications run on the same server
- TLS encryption happens before deciding which application we want to access
- In 2003 an extension was added to TLS: SNI - allowing to specify which domain we want to be access
- Older browser do not necessary support SNI. CFN needs to allocate dedicated IP addresses for these users, at extra charge
- CFN can be used in SNI mode (free) or allocating extra IP addresses ($600 per month)
- CloudFront SSL/SNI architecture:
    ![SSL/SNI architecture](images/CloudFrontSSLSNI.png)
- For S3 origin, we don't need to apply certificates for the origin protocol. For ALB/EC2/on-prem we can have public certificates which needs to match the DNS name of the origin

## Origin Types and Architecture

- Origins are the locations from where CF goes to get content
- If there is a cache miss in case of a request, than an origin fetch occurs
- Origin groups allow us to add resiliency. We can group origins together an have an origin group used by the behavior
- Categories of origins:
    - Amazon S3 buckets
    - AWS media package channel endpoint
    - AWS media store container endpoint
    - everything else (web-servers) - custom origins
- If S3 is configured to be used as a web-server, CF views it as a custom origin
- S3 origin configurations:
    - Origin Path: use a path instead of the top level of the bucket
    - Origin Access Identity: allows to give CF a virtual identity and use this to access the bucket
    - Origin Custom Headers
    - Viewer protocol policy is also used for the origin protocol
- Custom origin configurations:
    - Origin Path: point to an origin but use a sub-path
    - Minimum Origin SSL Protocol: best practice always to select the latest
    - Origin Protocol Policy: HTTP, HTTPS or Match Viewer
    - HTTP/HTTPS Port: we can use arbitrary port instead of 80 or 443
    - Origin Custom Headers: can be used for security to restrict access only from CF

## Caching Performance and Optimization

- Cache Hit: object is available in the cache in the edge location
- Cache Miss: object is not available in the cache, origin fetch is required
- Content retrieval techniques:
    - When we require an object from CFN, we usually request it using its name
    - We can use query string parameters as well, example `index.html&lang=en`
    - Cookies
    - Request Headers
- When using CFN all of this data reaches CloudFront first and than can be forwarded to the origin
- We can configure CFN to cache data based on some or all of these request properties
- When using CFN forward only the headers needed by the application and cache data based only on what can change the object
- The more things are involved in caching, the less efficient the process is

## CloudFront Security

### OAI and Custom Origins

- S3 origins:
    - OAI - Origin Access Identity: is a type of identity, it can be associated with CloudFront distributions
    - Essentially the CloudFront distributions "becomes" the OAI, meaning that this identity can be used in S3 bucket policies
    - Common pattern is to lock the S3 bucket to be only accessible to CloudFront
    - The edge location gains the attached OAI identity, meaning they will be able to access the bucket
    - Direct access from the end-user to the bucket content can be disabled
- Custom origins:
    - We can not use OAI to control access
    - We can utilize custom headers, which will be protected by the HTTPS protocol. CloudFront will be configured to send this custom header
    - Other way to handle CloudFront security from custom origins is to determine the IP ranges from which the request is coming from. CloudFront IP ranges are publicly available

### Private Distributions

- CloudFront can run in 2 different modes:
    - Public: can be accessed by any viewer
    - Private: requests to CloudFront needs to be made with a signed url or cookie
- If the CloudFront distribution has only 1 behavior the whole distribution is considered to be either public or private
- In case of multiple behaviors: each behavior can be either public or private
- In order to enable private distribution of content, we need to create a **CloudFront Key** by an Account Root User. That account is added as a **Trusted Signer**
- Signed URLs provide access to one particular object. They are also used for legacy RTMP distributions which can not use cookies
- Signed cookies can provide access to groups of objects or all files of a particular type

### CloudFront Geo Restriction

- Gives a way to restrict content to a particular location
- They are 2 types of restriction:
    - CloudFront Geo Restriction:
        - Whitelist or Blacklist countries
        - **Only works with countries!**
        - Uses a GeoIP database with 99.8% accuracy
        - Applies to the entire distribution
    - 3rd Party Geolocation:
        - Completely customizable, can be used to filter on lots of other attributes, example: username, user attributes, etc.
        - Requires an application server in front of CloudFront, which controls weather the customer has access to the content or not
        - The application generates a signed url/cookie which is returned to the browser. This can be sent to CloudFront for authorization

### Field-Level Encryption

- Field-Level encryption happens at the edge
- We can configure encryption using a public key for certain fields from the request
- Field-Level encryption happens separately from the HTTPS tunnel
- A private key is needed to decrypt individual fields
- Field-Level encryption architecture:
    ![Field-Level encryption architecture](images/FieldLevelEncryption2.png)

## Lambda@Edge

- Lambda@Edge allows us to run lightweight Lambda functions at the edge locations
- These Lambda functions allow us to adjust data between the viewer and the origin
- They don't have the full Lambda feature set:
    - Currently only NodeJS and Python are supported
    - Functions don't have access to any resources in a VPC, they run in AWS public space
    - Lambda Layers are not supported
- They have different size and duration limits compared to classic Lambda functions:
    - Viewer side: 128MB/5seconds
    - Origin side: same as classic Lambda/30seconds
- Lambda@Edge use cases:
    - A/B testing - viewer request function
    - Migration between S3 origins - origin request function
    - Different objects based on the type of device - origin request function
    - Content displayed by country - origin request
    - More examples: [https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-examples.html#lambda-examples-redirecting-examples](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-examples.html#lambda-examples-redirecting-examples)