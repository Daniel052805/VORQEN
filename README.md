# NextMesh AI Studio

A deployable Next.js starter for real Meshy AI text-to-3D and image-to-3D generation. This project calls Meshy's API from server-side route handlers; it does not fake generated models.

## Before you begin

- Node.js 20.9+ recommended.
- A Meshy developer account and an API key with access/credits for the endpoints you use.
- A GitHub account (for a simple Vercel deployment).

Official docs:
- Meshy quickstart: https://docs.meshy.ai/en/api/quick-start
- Text-to-3D: https://docs.meshy.ai/en/api/text-to-3d
- Image-to-3D: https://docs.meshy.ai/en/api/image-to-3d

## Run locally

1. Install Node.js from https://nodejs.org/ if you are using a computer.
2. Extract this ZIP.
3. Open a terminal in the `nextmesh-ai-studio` folder.
4. Install packages:

   ```bash
   npm install
   ```

5. Copy `.env.example` to `.env.local`.
6. Open `.env.local` and set your private key:

   ```env
   MESHY_API_KEY=your_real_key_here
   NEXTMESH_ACCESS_CODE=
   ```

   Do not put the API key in a file named `NEXT_PUBLIC_*`, and never commit `.env.local` to GitHub.

7. Start the site:

   ```bash
   npm run dev
   ```

8. Open http://localhost:3000

## Deploy online with Vercel

1. Create a GitHub repository at https://github.com/new.
2. Upload the extracted project files to the repository. Do not upload `.env.local` or any API key.
3. Open https://vercel.com and sign in with GitHub.
4. Select **Add New → Project**, then import the repository.
5. In Project Settings / Environment Variables, add:
   - `MESHY_API_KEY` = your Meshy API key
   - Optional: `NEXTMESH_ACCESS_CODE` = a long random passcode if you want a simple shared access gate
6. Deploy.
7. After adding or changing environment variables, redeploy the project.
8. Test text-to-3D and image-to-3D on the live URL.

Vercel's UI labels may change slightly over time. Use the official docs if needed: https://vercel.com/docs

## How the integration works

- Text-to-3D: creates a preview task using `/openapi/v2/text-to-3d`, polls it, then requests a refine task for texturing after preview succeeds.
- Image-to-3D: sends a JPG/PNG data URI to `/openapi/v1/image-to-3d` and polls the task.
- Status checks are server-side, and the Meshy key is never sent to the browser.
- The viewer uses Google's `<model-viewer>` component to render a real GLB model and provide camera controls.
- The API's generated asset URLs can expire; download assets you need to keep.

## Important starter limitations

This is a functional generation starter, not a complete commercial SaaS yet. It does not yet provide:
- User registration or login.
- Persistent database-backed project history.
- Multi-user ownership or individual quotas.
- Billing, subscriptions, or credit accounting.
- A production-grade per-user rate limiter.
- Durable storage of generated models.

Do not publish this publicly for unrestricted use until you add authentication, abuse protection, usage limits, and durable asset storage. The optional shared access code is a basic gate, not a replacement for proper user accounts. Serverless instance memory is not used for persistent data.

## Common errors

- **Missing `MESHY_API_KEY`:** add the variable in `.env.local` or hosting environment settings and restart/redeploy.
- **401 / unauthorized:** check the API key.
- **402 / payment required:** the account may lack credits or API entitlement.
- **429 / rate limit:** wait and retry according to the provider's policy.
- **Model is still processing:** wait; generation tasks are asynchronous and may take several minutes.
- **Text preview succeeds but refine fails:** inspect the provider message; refine may need additional credits or compatible model settings.

## Commands

```bash
npm run dev
npm run build
npm start
```

## Privacy and security

Never paste your Meshy API key into public chats, client-side code, browser console, screenshots, or Git commits. Configure it only in a private environment variable. Uploaded images are sent to Meshy for generation.
