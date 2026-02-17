# Explanation

## What was the bug?

The client did not refresh the OAuth2 token when `oauth2_token` was a dictionary instead of an `OAuth2Token` instance. As a result, API requests were sent without an Authorization header.

## Why did it happen?

The refresh condition only checked:
- If the token was None
- If it was an expired OAuth2Token

It did not handle invalid token types (like dict), so refresh was skipped.

## Why does your fix solve it?

The condition was updated to refresh whenever the token is not an instance of `OAuth2Token` or is expired. This ensures invalid types and expired tokens are both refreshed before making API requests.

## What edge case is not covered?

We do not test behavior when `refresh_oauth2()` itself fails or returns an invalid token. That failure path is not currently covered.
