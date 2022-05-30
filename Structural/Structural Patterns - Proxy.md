---
attachments: [factory.png]
tags: [Design Patterns]
title: Structural Patterns - Proxy
created: "2021-05-08T17:45:11.449Z"
modified: "2021-05-08T17:47:54.653Z"
---

// PROXY
// Intent:
// provides a substitute or placeholder for another object
// Design:
// text

# Structural Patterns - Proxy

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Structural patterns are concerned with how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.

Proxy lets you provide a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.

#### Objects

- **Service Interface** declares the interface of the Service. The proxy must follow this interface to be able to disguise itself as a service object.

- **Service** is a class that provides some useful business logic.

- **Proxy** class has a reference field that points to a service object. After the proxy finishes its processing (e.g., lazy initialization, logging, access control, caching, etc.), it passes the request to the service object.

Usually, proxies manage the full lifecycle of their service objects.

- **Client** should work with both services and proxies via the same interface. This way you can pass a proxy into any code that expects a service object.

<img src="https://refactoring.guru/images/patterns/diagrams/proxy/structure-indexed.png" width="500" />

#### Application

- Lazy initialization (virtual proxy). This is when you have a heavyweight service object that wastes system resources by being always up, even though you only need it from time to time.
- Access control (protection proxy). This is when you want only specific clients to be able to use the service object; for instance, when your objects are crucial parts of an operating system and clients are various launched applications (including malicious ones).
- Local execution of a remote service (remote proxy). This is when the service object is located on a remote server.
- Logging requests (logging proxy). This is when you want to keep a history of requests to the service object.
- Caching request results (caching proxy). This is when you need to cache results of client requests and manage the life cycle of this cache, especially if results are quite large.
- Smart reference. This is when you need to be able to dismiss a heavyweight object once there are no clients that use it.

#### Code

```typescript

// The interface of a remote service.
interface ThirdPartyYouTubeLib is
    method listVideos()
    method getVideoInfo(id)
    method downloadVideo(id)

// The concrete implementation of a service connector. Methods
// of this class can request information from YouTube. The speed
// of the request depends on a user's internet connection as
// well as YouTube's. The application will slow down if a lot of
// requests are fired at the same time, even if they all request
// the same information.
class ThirdPartyYouTubeClass implements ThirdPartyYouTubeLib is
    method listVideos() is
        // Send an API request to YouTube.

    method getVideoInfo(id) is
        // Get metadata about some video.

    method downloadVideo(id) is
        // Download a video file from YouTube.

// To save some bandwidth, we can cache request results and keep
// them for some time. But it may be impossible to put such code
// directly into the service class. For example, it could have
// been provided as part of a third party library and/or defined
// as `final`. That's why we put the caching code into a new
// proxy class which implements the same interface as the
// service class. It delegates to the service object only when
// the real requests have to be sent.
class CachedYouTubeClass implements ThirdPartyYouTubeLib is
    private field service: ThirdPartyYouTubeLib
    private field listCache, videoCache
    field needReset

    constructor CachedYouTubeClass(service: ThirdPartyYouTubeLib) is
        this.service = service

    method listVideos() is
        if (listCache == null || needReset)
            listCache = service.listVideos()
        return listCache

    method getVideoInfo(id) is
        if (videoCache == null || needReset)
            videoCache = service.getVideoInfo(id)
        return videoCache

    method downloadVideo(id) is
        if (!downloadExists(id) || needReset)
            service.downloadVideo(id)
// The GUI class, which used to work directly with a service
// object, stays unchanged as long as it works with the service
// object through an interface. We can safely pass a proxy
// object instead of a real service object since they both
// implement the same interface.
class YouTubeManager is
    protected field service: ThirdPartyYouTubeLib

    constructor YouTubeManager(service: ThirdPartyYouTubeLib) is
        this.service = service

    method renderVideoPage(id) is
        info = service.getVideoInfo(id)
        // Render the video page.

    method renderListPanel() is
        list = service.listVideos()
        // Render the list of video thumbnails.

    method reactOnUserInput() is
        renderVideoPage()
        renderListPanel()

// The application can configure proxies on the fly.
class Application is
    method init() is
        aYouTubeService = new ThirdPartyYouTubeClass()
        aYouTubeProxy = new CachedYouTubeClass(aYouTubeService)
        manager = new YouTubeManager(aYouTubeProxy)
        manager.reactOnUserInput()

        // The proxy pattern is a structural pattern that creates a proxy object. It acts as a placeholder for another object, controlling the access to it.

class Capital {
	getCapital(country) {
		if (country === "Pakistan") {
			return "Islamabad";
		} else if (country === "India") {
			return "new Delhi";
		} else if (contry === "Canada") {
			return "Ottawa";
		} else if (country === "Egypt") {
			return "Cairo";
		} else {
			return "Country not in database";
		}
	}
}

class ProxyCapital {
	constructor() {
		this.capital = new Capital();
		this.capitalMap = new Map();
	}

	getCapital(country) {
		if (!this.capitalMap.get(country)) {
			let result = this.capital.getCapital(country);
			this.capitalMap.set(country, result);
			return `${result} -- retrieved from target object.`;
		} else {
			let result = this.capitalMap.get(country);
			return `${result} -- retrieved from cache.`;
		}
	}
}

let newTarget = new Capital();
let newProxy = new ProxyCapital();

console.log(newProxy.getCapital("India"));
console.log(newProxy.getCapital("India"));

```
