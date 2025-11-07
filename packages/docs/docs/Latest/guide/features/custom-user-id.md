# Custom User ID

When calling most Dify APIs, you need to pass `userId` to identify the user.

By default, Dify Chat uses `FingerprintJS` to generate a unique user ID and passes it as the `userId` parameter.

If you want to control the generation logic of `userId`, you can modify the `mockLogin` function to customize the login logic:

```tsx title="packages/react-app/src/pages/auth/index.tsx"
/**
 * Mock login
 */
const mockLogin = async () => {
  const fp = await FingerPrintJS.load();
  const result = await fp.get();
  return await new Promise<{ userId: string }>((resolve) => {
    setTimeout(() => {
      resolve({
        userId: result.visitorId,
      });
    }, 2000);
  });
};
```
