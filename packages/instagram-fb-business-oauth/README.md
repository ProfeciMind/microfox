# @microfox/instagram-fb-business-oauth

A robust TypeScript SDK for Instagram and Facebook Business OAuth 2.0 authentication and API integration. This package provides a simple and type-safe way to handle OAuth flows for Instagram Business accounts through the Facebook Graph API v22.0, enabling seamless integration with Instagram's business features.

## Features

- 🔐 OAuth 2.0 authentication flow for Instagram Business accounts
- 🔄 Automatic token exchange and management
- 🔄 Long-lived token generation
- 📱 Instagram Business Account integration
- 🛡️ Type-safe implementation with Zod validation
- 📦 Zero dependencies (except for Zod)
- 🚀 Built with TypeScript for excellent developer experience
- 📝 Comprehensive TypeScript types and interfaces
- 🔒 Secure token handling and validation
- 🔄 Automatic scope management for Instagram Business features

## Installation

```bash
npm install @microfox/instagram-fb-business-oauth
# or
yarn add @microfox/instagram-fb-business-oauth
# or
pnpm add @microfox/instagram-fb-business-oauth
```

## Prerequisites

Before using this SDK, you need to:

1. Create a Facebook Developer account
2. Create a Facebook App
3. Get your Client ID and Client Secret
4. Configure Instagram API with Facebook Login
5. Set up OAuth redirect URIs

## Usage

### Basic Setup

```typescript
import {
  InstagramFbBusinessOAuthSdk,
  InstagramFbBusinessScope,
} from '@microfox/instagram-fb-business-oauth';

const oauth = new InstagramFbBusinessOAuthSdk({
  clientId: 'your-client-id',
  clientSecret: 'your-client-secret',
  redirectUri: 'https://your-app.com/callback',
  scopes: [
    InstagramFbBusinessScope.INSTAGRAM_BASIC,
    InstagramFbBusinessScope.INSTAGRAM_CONTENT_PUBLISH,
    InstagramFbBusinessScope.INSTAGRAM_MANAGE_COMMENTS,
    InstagramFbBusinessScope.INSTAGRAM_MANAGE_INSIGHTS,
    InstagramFbBusinessScope.PAGES_SHOW_LIST,
    InstagramFbBusinessScope.PAGES_READ_ENGAGEMENT,
  ],
});
```

### Available Scopes

The SDK provides the following scopes through the `InstagramFbBusinessScope` enum:

- `INSTAGRAM_BASIC`: Access basic Instagram profile information
- `INSTAGRAM_CONTENT_PUBLISH`: Publish content to Instagram
- `INSTAGRAM_MANAGE_COMMENTS`: Manage comments on Instagram posts
- `INSTAGRAM_MANAGE_INSIGHTS`: Access Instagram insights and analytics
- `PAGES_SHOW_LIST`: Access list of Facebook Pages
- `PAGES_READ_ENGAGEMENT`: Read engagement data for Facebook Pages

Note: The SDK automatically includes all necessary scopes for Instagram Business features.

### OAuth Flow

1. Generate Authorization URL:

```typescript
const authUrl = oauth.getAuthUrl();
// Redirect user to this URL
```

2. Handle the callback and exchange code for tokens:

```typescript
// In your callback route handler
const { accessToken, expiresIn, dataAccessExpirationTime } =
  await oauth.exchangeCodeForTokens(code);
```

3. Get a long-lived token:

```typescript
const {
  accessToken: longLivedToken,
  tokenType,
  expiresIn,
} = await oauth.getLongLivedToken(shortLivedToken);
```

4. Get Instagram Business Account details:

```typescript
const businessAccount = await oauth.getInstagramBusinessAccount(accessToken);
// Returns: {
//   pageId: string;
//   pageName: string;
//   pageAccessToken: string;
//   instagramBusinessAccountId: string;
// }
```

## API Reference

### Constructor

```typescript
new InstagramFbBusinessOAuthSdk(config: InstagramFbBusinessAuthConfig)
```

#### Config Options

- `clientId`: string (required) - Your Facebook App ID
- `clientSecret`: string (required) - Your Facebook App Secret
- `redirectUri`: string (required) - Your OAuth callback URL
- `scopes`: string[] (optional) - Additional scopes to request
- `state`: string (optional) - Custom state parameter for security

### Methods

#### getAuthUrl()

Returns the authorization URL to redirect users to.

#### exchangeCodeForTokens(code: string)

Exchanges an authorization code for access tokens. Returns:

```typescript
{
  accessToken: string;
  expiresIn: number;
  dataAccessExpirationTime: number | null;
}
```

#### getLongLivedToken(shortLivedToken: string)

Converts a short-lived token into a long-lived token. Returns:

```typescript
{
  accessToken: string;
  tokenType: string;
  expiresIn: number;
}
```

#### getInstagramBusinessAccount(accessToken: string)

Retrieves the Instagram Business Account associated with the access token. Returns:

```typescript
{
  pageId: string;
  pageName: string;
  pageAccessToken: string;
  instagramBusinessAccountId: string;
}
```

## Error Handling

The SDK throws descriptive errors for various scenarios:

- Invalid configuration
- OAuth flow errors
- API request failures
- Validation errors

All errors include detailed error messages and, when available, error descriptions from the Facebook API. The SDK uses Zod for runtime validation of API responses.

## Security Considerations

- Always use HTTPS for your redirect URI
- Store tokens securely
- Implement proper state validation
- Keep your client secret secure
- Use environment variables for sensitive data
- Regularly rotate your client secret
- Monitor token expiration and refresh tokens as needed
- Implement proper error handling and logging
- Use secure storage for tokens and credentials

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. Before contributing, please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run tests
5. Submit a pull request

## License

MIT © [TheMoonDevs](https://github.com/microfox-ai/microfox)
