# Vercel Deployment Instructions

## Environment Variables

Make sure to set these environment variables in your Vercel project settings:

### Required Variables
- `NODE_ENV=production`

### Optional Variables
- `USE_WORKER_THREADS=false` (Set to false for Vercel - worker threads don't work in serverless)
- `ALLOWED_ORIGINS=*` (Or specify your domains)
- `ALLOWED_DOMAINS` (Comma-separated list of allowed domains to proxy)
- `ENABLE_DOMAIN_WHITELIST=false` (Set to true if using domain whitelist)
- `MAX_REQUEST_SIZE=10485760` (10MB)
- `REQUEST_TIMEOUT=30000` (30 seconds - Vercel has max 10s for Hobby, 60s for Pro)
- `LOG_LEVEL=info`

## Important Notes for Vercel Deployment

1. **Worker Threads**: Must be disabled (`USE_WORKER_THREADS=false`) as Vercel serverless functions don't support worker threads
2. **Timeout Limits**: 
   - Hobby plan: 10 seconds max
   - Pro plan: 60 seconds max
   - Set `REQUEST_TIMEOUT` accordingly
3. **Memory**: Vercel functions have 1024MB memory limit (3008MB on Pro)
4. **Cold Starts**: First request after idle period will be slower

## Deployment Steps

1. Install Vercel CLI (if not already installed):
   ```bash
   npm i -g vercel
   ```

2. Deploy to Vercel:
   ```bash
   vercel
   ```

3. Set environment variables in Vercel dashboard or via CLI:
   ```bash
   vercel env add NODE_ENV production
   vercel env add USE_WORKER_THREADS false
   ```

4. Redeploy after setting environment variables:
   ```bash
   vercel --prod
   ```

## Troubleshooting

If you get `FUNCTION_INVOCATION_FAILED` error:
- Check Vercel function logs: `vercel logs <deployment-url>`
- Ensure all dependencies are in `dependencies` (not `devDependencies`)
- Verify environment variables are set correctly
- Check that `USE_WORKER_THREADS` is set to `false`
