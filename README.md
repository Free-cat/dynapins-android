# Dynapins Android

Android SDK for dynamic SSL/TLS certificate pinning with OkHttp integration.

## 🚧 Coming Soon

This project is under development.

## 📋 Planned Features

- **OkHttp Integration**
  - Custom Interceptor
  - Automatic certificate validation
  - Proxy support
  
- **Certificate Validation**
  - Public key pinning
  - Certificate fingerprint verification
  - Ed25519 signature validation
  
- **Caching & Performance**
  - Local certificate cache
  - Automatic updates from server
  - Offline support
  
- **Easy Integration**
  - Maven Central distribution
  - Minimal dependencies
  - Kotlin-first API

## 🎯 Planned API

```kotlin
import com.freecats.dynapins.Dynapins

// Initialize
val dynapins = Dynapins(
    serverUrl = "https://api.example.com",
    publicKey = "your-ed25519-public-key"
)

// Configure OkHttpClient
val client = OkHttpClient.Builder()
    .addInterceptor(dynapins.createInterceptor())
    .build()

// Make requests - pinning is automatic
val request = Request.Builder()
    .url("https://secure.example.com")
    .build()

client.newCall(request).execute().use { response ->
    // Handle response
}
```

### Advanced Usage

```kotlin
// Manual certificate validation
dynapins.validateCertificate("secure.example.com") { result ->
    when (result) {
        is Result.Success -> println("Certificate valid: ${result.data}")
        is Result.Failure -> println("Validation failed: ${result.error}")
    }
}

// Check cache status
val cached = dynapins.getCachedCertificate("example.com")
if (cached != null) {
    println("Using cached certificate")
}

// Force refresh
dynapins.refreshCertificates { result ->
    println("Certificates updated")
}

// Configure custom cache
val dynapins = Dynapins(
    serverUrl = "https://api.example.com",
    publicKey = "...",
    cacheConfig = CacheConfig(
        maxAge = Duration.ofHours(24),
        maxSize = 100
    )
)
```

## 🔧 Requirements

- Android 6.0+ (API 23+)
- Kotlin 1.9+
- OkHttp 4.12+

## 📦 Installation

### Gradle (Kotlin DSL)

```kotlin
dependencies {
    implementation("com.freecats:dynapins:0.0.1")
}
```

### Gradle (Groovy)

```groovy
dependencies {
    implementation 'com.freecats:dynapins:0.0.1'
}
```

## 🔒 Permissions

Add to your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## 🔗 Related Projects

- [Dynapins Server](../dynapins-server) - Backend HTTP API
- [Dynapins iOS](../dynapins-ios) - iOS SDK

## 📞 Contributing

Interested in contributing? We'd love your help building this!

Check out our [contribution guidelines](../dynapins-server/CONTRIBUTING.md).

## 📄 License

MIT License - see [LICENSE](../dynapins-server/LICENSE) for details.
