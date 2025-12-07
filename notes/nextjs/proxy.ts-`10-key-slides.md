1. **Proxy.js replaces middleware.ts** - Next.js 16.0.7 renames middleware to proxy for clarity and better purpose alignment
```typescript
// Before: middleware.ts
export function middleware(request: NextRequest) { }

// After: proxy.ts
export function proxy(request: NextRequest) { }
```

2. **Server-side execution before rendering** - Proxy runs on the server before requests complete, enabling authentication, logging, and redirects
```typescript
export function proxy(request: NextRequest) {
  console.log('Request:', request.url)
  // Runs before any route renders
  return NextResponse.next()
}
```

3. **Single function export required** - Export one proxy function (default or named) from proxy.ts in your project root
```typescript
// Option 1: Named export
export function proxy(request: NextRequest) { }

// Option 2: Default export
export default function(request: NextRequest) { }
```

4. **Matcher for precise path control** - Use the matcher config to specify which paths trigger your proxy logic
```typescript
export const config = {
  matcher: ['/about/:path*', '/dashboard/:path*', '/api/:path*']
}
```

5. **NextRequest and NextResponse APIs** - Access request data and modify responses through convenient Next.js extensions
```typescript
export function proxy(request: NextRequest) {
  const url = request.nextUrl
  const response = NextResponse.next()
  return response
}
```

6. **Cookie and header manipulation** - Easily read and set cookies/headers using built-in NextRequest/NextResponse methods
```typescript
export function proxy(request: NextRequest) {
  const token = request.cookies.get('token')
  const response = NextResponse.next()
  response.cookies.set('session', 'value')
  response.headers.set('x-custom-header', 'value')
  return response
}
```

7. **CORS configuration support** - Set CORS headers in proxy to handle simple and preflighted cross-origin requests
```typescript
export function proxy(request: NextRequest) {
  const response = NextResponse.next()
  response.headers.set('Access-Control-Allow-Origin', '*')
  response.headers.set('Access-Control-Allow-Methods', 'GET, POST, PUT')
  return response
}
```

8. **Direct response capability** - Return Response or NextResponse instances directly from proxy without rendering routes
```typescript
export function proxy(request: NextRequest) {
  if (request.nextUrl.pathname === '/blocked') {
    return new Response('Access denied', { status: 403 })
  }
  return NextResponse.next()
}
```

9. **Node.js runtime only** - Proxy defaults to Node.js runtime; Edge Runtime configuration is not supported
```typescript
// ❌ This will throw an error
export const config = {
  runtime: 'edge' // Not supported in proxy
}

// ✅ Proxy uses Node.js runtime by default
export function proxy(request: NextRequest) { }
```

10. **Migration codemod available** - Run npx @next/codemod@canary proxy-migration to automatically migrate from middleware.ts
```bash
# Run this command in your project root
npx @next/codemod@canary proxy-migration

# Automatically renames middleware.ts to proxy.ts
# and updates function names
```
