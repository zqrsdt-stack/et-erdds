/**
 * Cloudflare Worker — Instagram Profile Proxy
 *
 * يرسل طلبات Instagram من Cloudflare edge IPs بدل Replit datacenter IPs.
 * مجاني: 100,000 طلب/يوم.
 *
 * نشر Worker:
 *   1. روح https://dash.cloudflare.com → Workers & Pages → Create application → Create Worker
 *   2. انسخ هذا الكود واحفظ
 *   3. انسخ رابط الـ Worker (مثال: https://ig-proxy.USERNAME.workers.dev)
 *   4. أضفه في Replit Secrets باسم: CF_IG_PROXY_URL
 */

const CLASSIC_UAS = [
  'Instagram 76.0.0.15.395 Android (24/7.0; 380dpi; 1080x1920; OnePlus; ONEPLUS A3010; OnePlus3T; qcom; en_US)',
  'Instagram 101.0.0.15.120 Android (26/8.0.0; 480dpi; 1080x1920; Samsung; SM-G950F; dreamlte; samsungexynos8895; en_US)',
  'Instagram 123.0.0.21.114 Android (26/8.0.0; 480dpi; 1080x1920; HUAWEI/huawei; BLA-L29; HWBLA; hi3660; en_US; 188178876)',
];

export default {
  async fetch(request) {
    const url = new URL(request.url);
    const username = url.searchParams.get('username');

    if (!username || !/^[a-zA-Z0-9._]{1,30}$/.test(username)) {
      return new Response(JSON.stringify({ error: 'invalid username' }), {
        status: 400,
        headers: { 'Content-Type': 'application/json' },
      });
    }

    const ua = CLASSIC_UAS[Math.floor(Math.random() * CLASSIC_UAS.length)];

    const igRes = await fetch(
      `https://i.instagram.com/api/v1/users/web_profile_info/?username=${encodeURIComponent(username)}`,
      {
        headers: {
          'User-Agent':      ua,
          'Accept':          'application/json',
          'Accept-Language': 'en-US,en;q=0.9',
        },
      }
    );

    const body = await igRes.text();
    return new Response(body, {
      status: igRes.status,
      headers: {
        'Content-Type': 'application/json',
        'Access-Control-Allow-Origin': '*',
      },
    });
  },
};
