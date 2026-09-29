# Auth Testing Playbook — Academia de Investidores

## Test Accounts

### Emergent Google Auth
- Tested via redirect flow to `https://auth.emergentagent.com/?redirect=<origin>/perfil`
- On success, frontend receives `#session_id=...` fragment
- Frontend POSTs to `/api/auth/session` which sets `session_token` httpOnly cookie
- Backend accepts both `session_token` cookie AND `Authorization: Bearer <jwt>` header

### JWT email/password (existing)
- POST `/api/auth/register` with `{name, email, password}` → returns token+user
- POST `/api/auth/login` with `{email, password}` → returns token+user
- Sample user: email `teste2@ai.com`, password `12345678`

## Manual test session setup (Emergent flow)
```bash
mongosh --eval "
use('test_database');
var userId = 'user_' + Math.random().toString(36).slice(2,14);
var sessionToken = 'test_session_' + Date.now();
db.users.insertOne({
  id: userId,
  email: 'test.google.' + Date.now() + '@example.com',
  name: 'Test Google User',
  picture: 'https://via.placeholder.com/150',
  auth_provider: 'google',
  xp: 0, level: 1, completed_modules: [], medals: [],
  created_at: new Date().toISOString()
});
db.user_sessions.insertOne({
  user_id: userId,
  session_token: sessionToken,
  expires_at: new Date(Date.now() + 7*24*60*60*1000).toISOString(),
  created_at: new Date().toISOString()
});
print('SESSION: ' + sessionToken);
"
```

## Endpoints
- POST `/api/auth/session` — Body/Header `X-Session-ID`, exchanges Emergent session_id, sets cookie, returns user
- GET `/api/auth/me` — Reads session_token cookie OR JWT bearer token
- POST `/api/auth/logout` — Clears session
- POST `/api/auth/register` (JWT) — legacy
- POST `/api/auth/login` (JWT) — legacy

## Success Indicators
- ✅ `/api/auth/me` returns user data after any auth type
- ✅ `session_token` cookie set with httpOnly + secure + samesite=none
- ✅ Frontend detects `#session_id=` on load and redirects seamlessly
- ✅ Logout clears session cookie AND DB record
