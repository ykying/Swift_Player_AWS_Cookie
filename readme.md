# iOS Swift AVPlayer with AWS_Cookie (Private content)
Simple code for how to use AVPlayer with AWS's private content.

```swift
// Sample cookies JSON
/*
"cookie": {
    "CloudFront-Policy": "AAA",
    "CloudFront-Key-Pair-Id": "BBB",
    "CloudFront-Signature": "CCC"
}
*/

// Build cookies
let url = URL(string: "https://YOUR_VIDEO_URL.m3u8");
var cookies = [HTTPCookie]()
    if let cookie = json["cookie"] as? [String: String] {
    for key in cookie.keys {
        let cookieField = ["Set-Cookie": "\(key)=\(cookie[key] ?? "")"]
        let cookie = HTTPCookie.cookies(withResponseHeaderFields: cookieField, for: url)
        cookies.append(contentsOf: cookie)
    }
}

// Assign cookies to AVPlayer
let values = HTTPCookie.requestHeaderFields(with: cookies)
let cookieOptions = ["AVURLAssetHTTPHeaderFieldsKey": values]
let assets = AVURLAsset(url: url, options: cookieOptions)
let item = AVPlayerItem(asset: assets)
let player = AVPlayer(playerItem: item)

//Have fun :)
```
