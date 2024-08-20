```javascript
type HTTPMethod = 'GET' | 'PUT' | 'PATCH' | 'POST' | 'DELETE';

  

class API {
	readonly method: HTTPMethod;
	readonly url: string;
	baseURL?: string;
	data?: unknown;

	constructor(method: HTTPMethod, url: string) {
		this.method = method;
		this.url = url;
	}
}

  

class APIBuilder {
	private _instance: API;

	constructor(method: HTTPMethod, url: string, data?: unknown) {
		this._instance = new API(method, url);
		this._instance.baseURL = url;
		this._instance.data = data;
	}

	static get = (url: string) => new APIBuilder('GET', url);
	static put = (url: string) => new APIBuilder('PUT', url);
	static patch = (url: string) => new APIBuilder('PATCH', url);
	static post = (url: string) => new APIBuilder('POST', url);
	static delete = (url: string) => new APIBuilder('DELETE', url);

	baseURL(value: string): APIBuilder {
		this._instance.baseURL = value;
		return this;
	}

  

	data(value: unknown): APIBuilder {
		this._instance.data = value;
		return this;
	}

  

	build(): API {
		return this._instance;
	}
}

  

const fetchTest = async (name?: string, size?: number) => {
	const api = APIBuilder.get('/apis/web/jobs')
				.baseURL('localhost:8080')
				.build();
};
```
