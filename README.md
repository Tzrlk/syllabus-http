# Syllabus HTTP

This is the HTTP implementation of the Syllabus event bus. It provides POST support for endpoints, referral logging,
and a context service that provides access to the http request and response objects for the given thread.

## Implementation

Unless you require access to the HTTP context, you should only need to include syllabus-http in your Maven pom.
Otherwise you need to inject the service as you would any other Univeristy Spring Component:

    @Inject
    HttpContextService httpContextService;

This should really only be necessary for setting response status codes, content types, or header information. Ideally,
it shouldn't be necessary, and it makes your code less portable.

## Future Development

There are a number of things that should be taken into account when using this library, and areas of improvement that
should be visited at a later date, but before further development makes implementing them more difficult.

### Header Information

Request and response header information should be the only real reasons to require the HTTP context. Since the
BaseRequest and BaseResponse objects in syllabus-core can support a "headers" object, it should be fairly simple to
populate this object with whatever header information is deemed useful to endpoints (cache headers, etc.). Further,
things like response codes, content types and disposition (for file downloads) can be added and modified as optional
properties.

### Exception Handling

The exception handler in this module was originally designed to render an HTTP-500 response on uncaught exceptions
being thrown. With the advent of the ErrorResponse class in syllabus-core, this no-longer became the preferred way of
handling error responses. As it serves no http-specific purpose, it can be removed, with relevant code being merged
into syllabus-core.