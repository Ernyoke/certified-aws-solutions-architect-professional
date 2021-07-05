# CloudFront

- It is a content deliver network (CDN)
- Its job is to improve the delivery of content from its original location to the viewers of the content
- It is accomplishing this by caching and by using an efficient global network

## CloudFrom Terms and Architecture

- **Origin**: the source location of the content, can be S3 or custom origin (publicly routable IPv4 address)
- **Distribution**: unit of configuration within CloudFront, which gets deployed out to the CloudFront network. Almost everything is configured within the distribution
- **Edge Location**: pieces of global infrastructure where the content is cached. They are smaller than AWS regions, but there are way more in number. Can be used to distribute static data only
- **Regional Edge Cache**: larger version of an edge location. Provides another layer of caching
- CloudFront Architecture:
    ![CloudFront Architecture](images/CloudFrontArchitecture1.png)
- If we are using S3 origins, the region edge location is not used if there is a cache miss for an edge location. Only custom origin can use the regional edge cache!
- **Origin fetch**: the content is fetched from the origin in case of a cache miss on the edge location
- **Behavior**: it is configuration within a distribution. Origins are directly linked to behaviors, behaviors are linked to distributions
    ![CloudFront Behavior](images/CloudFrontArchitecture2.png)