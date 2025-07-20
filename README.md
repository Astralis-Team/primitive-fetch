# 🔮 Primitive-fetch

A primitive HTTP client for making API requests.

## Installation

```bash
npm install @astralis-team/primitive-fetch
```

## Usage

```typescript
import fetches from '@astralis-team/fetch'

const response = await fetches.get<User[]>('/users')
```

## Custom instance

```typescript
const $api = pfetch.create({
	baseURL: 'https://api.example.com',
	headers: {
		'Content-Type': 'application/json',
	},
})

$api.interceptors.request.use(
	config => {
		config.headers.Authorization = 'Bearer token'
		return config
	},
	error => Promise.reject(error)
)
```

## Request wrapper

```typescript
interface GetUsersParams {
	limit: number
	page: number
}

const getUsers: ApiFetchesRequest<GetUsersParams, any[]> = ({
	params,
	config,
}) =>
	pfetch.get('/users', {
		...config,
		params: { ...config?.params, ...params },
	})
```

## Response parse

The fetches library provides flexible response parsing options to handle different types of responses. You can specify how to parse the response body using predefined modes or custom functions. If no parse mode is specified, the response will be automatically parsed based on the `Content-Type` header:

```typescript
const response = await pfetch.get('/users', { parse: 'json' })
```

### Custom parse functions

You can provide a custom function that takes a Response object and returns a Promise with parsed data. This is useful for handling special response formats or custom parsing logic.

```typescript
const response = await pfetch.get('/users', { parse: data => data.json() })
```

### Raw response

To get the raw response body without any parsing, use the `'raw'` parse mode. This is useful when you need to handle the response manually or when working with binary data.

```typescript
const response = await pfetch.get('/binary-file', { parse: 'raw' })
```
