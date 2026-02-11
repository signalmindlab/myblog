# Cloudflare Pages Deployment Setup

## The Issue
Your Cloudflare Pages deployment was failing because:
- ❌ The deploy command was trying to run `npx wrangler deploy`
- ❌ This command requires a Worker script or specific Wrangler configuration
- ❌ Your project is a Next.js static site, not a Cloudflare Worker

## The Solution

Your Cloudflare Pages build settings should be:

### In Cloudflare Dashboard:
1. Go to **Pages** → Your Project (blog.drabdus.info)
2. Click **Settings** → **Build and Deployment**
3. Under **Build settings**, configure:

| Setting | Value |
|---------|-------|
| **Build command** | `yarn build` |
| **Build output directory** | `.next` |
| **Root directory** | (leave blank) |
| **Deploy command** | (leave BLANK - remove if exists) |

4. Click **Save and Deploy** or trigger a manual deployment

## Why This Works

1. **Build command:** `yarn build`
   - Runs your Next.js build process
   - Generates output in `.next` directory

2. **Build output directory:** `.next`
   - Tells Cloudflare Pages where to find your built files
   - Cloudflare will serve these static files directly

3. **No Deploy command needed**
   - Your site is static content, not a Worker
   - Cloudflare Pages handles deployment automatically

## Files Included

- `wrangler.toml` - Optional Cloudflare configuration file (helps with clarity)
- This guide - For reference

## Deployment Flow

```
1. Push to main branch
   ↓
2. Cloudflare Pages webhook triggered
   ↓
3. Runs: yarn build
   ↓
4. Output generated in .next/
   ↓
5. Cloudflare serves from .next/
   ↓
6. Site live at blog.drabdus.info ✅
```

## Testing

After fixing the configuration:
1. Make a small change to your site
2. Commit and push to main
3. Check Cloudflare Pages dashboard for build progress
4. Wait for "Success" status
5. Visit https://blog.drabdus.info to verify

## Troubleshooting

**If still getting 404:**
- ✅ Check build output directory is set to `.next`
- ✅ Ensure no deploy command is set
- ✅ Check build command is `yarn build`
- ✅ Verify build completed successfully in Cloudflare dashboard

**Build fails with "missing dependencies":**
- Run `yarn install` locally to verify
- Check `.yarnrc.yml` is correct
- May need to increase build timeout in settings

**Build succeeds but site still shows 404:**
- Clear Cloudflare cache
- Hard refresh browser (Ctrl+Shift+R)
- Check that `.next` folder exists after build
