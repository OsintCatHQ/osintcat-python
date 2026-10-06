# Reference
## Account
<details><summary><code>client.account.<a href="src/osintcat/account/client.py">get</a>() -> UserResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the account the API key belongs to: plan, plan expiry, and how many lookups are left today.

Does not count as a lookup.

Needs the `account:read` scope (keys with all scopes have it).

Errors:
- 401 `API key required`: No `X-API-KEY` header.
- 403 `Invalid API key`: The key does not exist or was revoked.
- 403 `insufficient_scope`: The key lacks the `account:read` scope.

Docs: https://docs.osintcat.net/api-reference/endpoint/user
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.account.get()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.account.<a href="src/osintcat/account/client.py">modules</a>() -> ModulesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists every module with its current state and whether your account can use it. Use it to check access before calling a lookup: a lookup for a module your plan does not include is refused with `403 ACCESS_DENIED`.

Does not count as a lookup.

Needs the `osint:read` scope.

Errors:
- 401 `Unauthorized`: Missing or unknown key.

Docs: https://docs.osintcat.net/api-reference/endpoint/modules
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.account.modules()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Breach
<details><summary><code>client.breach.<a href="src/osintcat/breach/client.py">search</a>(...) -> BreachResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Searches the breach indexes (including Snusbase, LeakCheck and IntelX) for a value and returns every matching record. The search is case-insensitive.

Results for the same query may come from a cache for up to two days.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/breach
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.breach.search(
    query="user@example.com",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — What to search for: an e-mail address, username, domain, phone number, IP address, name or password.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.breach.<a href="src/osintcat/breach/client.py">database_search</a>(...) -> DatabaseSearchResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Searches stealer-log and combo-list collections for an e-mail address or a domain.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 424 `Upstream error`: The search backend answered with an error. Not charged.
- 424 `timeout error`: The search backend did not answer in time. Not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/database-search
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.breach.database_search(
    query="user@example.com",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — The e-mail address or domain.
    
</dd>
</dl>

<dl>
<dd>

**query_type:** `typing.Optional[DatabaseSearchBreachRequestType]` — `email` or `domain`. Detected from `query` when left out.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.breach.<a href="src/osintcat/breach/client.py">domain</a>(...) -> DomainResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns breach records whose e-mail addresses or URLs belong to a domain.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, requests are refused with `429 LIMIT_REACHED` until it resets at 00:00 UTC.

Errors:
- 404 `No results found`: Nothing was found for the domain.
- 424 `Upstream error`: The search could not be completed; the response carries an `error_id`.

Docs: https://docs.osintcat.net/api-reference/endpoint/domain
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.breach.domain(
    query="example.com",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — The domain, e.g. `example.com`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Email
<details><summary><code>client.email.<a href="src/osintcat/email/client.py">lookup</a>(...) -> EmailOsintResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Checks which websites and services an e-mail address is registered with and in which breaches it appears. Every request must state its purpose.

A request without a purpose (parameter `purpose`, header `X-Purpose`, or `Purpose: ...` at the end of your User-Agent) is refused with `400 USER_AGENT_IDENTITY_REQUIRED`.

Does not use your daily allowance. Each lookup that finds something is charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing, or fails, is not charged.

Errors:
- 400 `USER_AGENT_IDENTITY_REQUIRED`: No purpose given.
- 401 `API key required`: No `X-API-KEY` header.
- 402 `INSUFFICIENT_BALANCE`: Your balance does not cover the lookup.
- 424 `Provider Error`: The lookup could not be completed. Not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/email-osint
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.email.lookup(
    query="user@example.com",
    purpose="fraud prevention",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — The e-mail address (`email` is accepted as well).
    
</dd>
</dl>

<dl>
<dd>

**purpose:** `str` — Why you run the lookup, e.g. `fraud prevention`. Required unless sent as a header (below).
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Phone
<details><summary><code>client.phone.<a href="src/osintcat/phone/client.py">lookup</a>(...) -> PhoneOsintResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns what is known about a phone number: whether it is valid, the carrier and line type, the country, linked online accounts and a risk score.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `phone_*`: The number cannot be a valid phone number; `code` says why (e.g. `phone_too_short`).
- 424 `Upstream provider error`: The lookup could not be completed.
- 424 `The request timed out.`: The lookup took too long.

Docs: https://docs.osintcat.net/api-reference/endpoint/phone-osint
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.phone.lookup(
    query="+4915112345678",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — The phone number in international format, e.g. `+4915112345678`. Spaces and dashes are allowed.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Ip
<details><summary><code>client.ip.<a href="src/osintcat/ip/client.py">lookup</a>(...) -> IpResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns what is known about an IP address: open ports and services seen on it, hostnames, and its location and network.

Results for the same address may come from a cache for up to two days.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/ip
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.ip.lookup(
    query="8.8.8.8",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — An IPv4 or IPv6 address.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Dns
<details><summary><code>client.dns.<a href="src/osintcat/dns/client.py">resolve</a>(...) -> DnsResolverResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Resolves one or more hostnames to their IP addresses.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/dns-resolver
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.dns.resolve(
    query="example.com,osintcat.net",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — A hostname, or several separated by commas.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Minecraft
<details><summary><code>client.minecraft.<a href="src/osintcat/minecraft/client.py">player</a>(...) -> MinecraftResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Combines the Minecraft profile of a player (UUID, name history, capes, skins) with records about them found in Minecraft server leaks.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/minecraft
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.minecraft.player(
    query="Notch",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — A Minecraft username (or the value matching `type`).
    
</dd>
</dl>

<dl>
<dd>

**query_type:** `typing.Optional[PlayerMinecraftRequestType]` — What `query` is for the leak search: `username` (default), `uuid`, `email` or `ip`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.minecraft.<a href="src/osintcat/minecraft/client.py">leaks</a>(...) -> MinecraftOsintResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Searches Minecraft server leaks for one value. Use [Minecraft Player](https://docs.osintcat.net/api-reference/endpoint/minecraft) for a profile plus leaks in one call.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `invalid query type`: `type` is missing or not one of the allowed values; `allowed_types` lists them.
- 424 `Upstream returned an empty response`: The search could not be completed; the response carries an `error_id`.

Docs: https://docs.osintcat.net/api-reference/endpoint/minecraft-osint
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.minecraft.leaks(
    query="Notch",
    query_type="username",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — The value to search for.
    
</dd>
</dl>

<dl>
<dd>

**query_type:** `LeaksMinecraftRequestType` — One of `username`, `uuid`, `email`, `ip`, `password`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.minecraft.<a href="src/osintcat/minecraft/client.py">profile</a>(...) -> MinecraftProfileResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Look up a Minecraft account by its username and get the public profile.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `Missing username parameter`: No `username` given.

Docs: https://docs.osintcat.net/api-reference/endpoint/minecraft-profile
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.minecraft.profile(
    username="Notch",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**username:** `str` — The Minecraft username.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Steam
<details><summary><code>client.steam.<a href="src/osintcat/steam/client.py">profile</a>(...) -> SteamResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Look up a Steam account by its username and get the public profile.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `Missing username parameter`: No `username` given.

Docs: https://docs.osintcat.net/api-reference/endpoint/steam
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.steam.profile(
    username="gaben",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**username:** `str` — The Steam username.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Xbox
<details><summary><code>client.xbox.<a href="src/osintcat/xbox/client.py">profile</a>(...) -> XboxResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Look up a Xbox account by its username and get the public profile.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `Missing username parameter`: No `username` given.

Docs: https://docs.osintcat.net/api-reference/endpoint/xbox
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.xbox.profile(
    username="Major Nelson",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**username:** `str` — The Xbox username.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Twitch
<details><summary><code>client.twitch.<a href="src/osintcat/twitch/client.py">profile</a>(...) -> TwitchResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Look up a Twitch account by its username and get the public profile.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `Missing username parameter`: No `username` given.

Docs: https://docs.osintcat.net/api-reference/endpoint/twitch
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.twitch.profile(
    username="xqc",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**username:** `str` — The Twitch username.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Chess
<details><summary><code>client.chess.<a href="src/osintcat/chess/client.py">lookup</a>(...) -> ChessResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the Chess.com profile of a player (ratings, clubs) and any records about the account found in the Chess.com leak.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/chess
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.chess.lookup(
    query="hikaru",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — A Chess.com username or an e-mail address.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Github
<details><summary><code>client.github.<a href="src/osintcat/github/client.py">profile</a>(...) -> GithubResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Look up a GitHub account by its username and get the public profile.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `Missing username parameter`: No `username` given.

Docs: https://docs.osintcat.net/api-reference/endpoint/github
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.github.profile(
    username="octocat",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**username:** `str` — The GitHub username.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Reddit
<details><summary><code>client.reddit.<a href="src/osintcat/reddit/client.py">profile</a>(...) -> RedditResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Look up a Reddit account by its username and get the public profile.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `Missing username parameter`: No `username` given.

Docs: https://docs.osintcat.net/api-reference/endpoint/reddit
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.reddit.profile(
    username="unidan",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**username:** `str` — The Reddit username.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## X
<details><summary><code>client.x.<a href="src/osintcat/x/client.py">profile</a>(...) -> TwitterResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the public profile of an X (Twitter) account and an automated, best-effort summary of its recent public posts (topics, language, tone, posting pattern). Treat the summary as unverified.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 404 `User '...' not found on X (Twitter).`: No account with that username. Not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/twitter
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.x.profile(
    query="jack",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — The X username, with or without `@`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Tiktok
<details><summary><code>client.tiktok.<a href="src/osintcat/tiktok/client.py">resolve_share_link</a>(...) -> TiktokResolverResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Resolves a TikTok share link (e.g. `https://vm.tiktok.com/...`) to the account that shared it, with share details and the video.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `Provide a valid TikTok short link via ?link=...`: No link, or not a TikTok link.
- 404 `No user found for this link`: The link carries no sharer.
- 424 `Could not resolve link`: The link could not be resolved right now.

Docs: https://docs.osintcat.net/api-reference/endpoint/tiktok-resolver
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.tiktok.resolve_share_link(
    link="https://vm.tiktok.com/ZMexample/",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**link:** `str` — A TikTok share link. `url` or `query` are accepted as well.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Instagram
<details><summary><code>client.instagram.<a href="src/osintcat/instagram/client.py">resolve_share_link</a>(...) -> InstagramResolverResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Resolves an Instagram post or reel share link to the account that shared it and the author of the post. Profile links cannot be resolved.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `Provide a valid Instagram URL via ?link=...`: No link, not an Instagram link, or the link expired or points to a private post. Not charged.
- 422 `profile_link`: A profile link: only post and reel share links can be resolved. Not charged.
- 424 `(message)`: The link could not be resolved right now. Not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/instagram-resolver
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.instagram.resolve_share_link(
    link="https://www.instagram.com/reel/Cexample/?igsh=MWexample",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**link:** `str` — An Instagram post or reel share link (with its `igsh` parameter). `url` or `query` are accepted as well.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Vin
<details><summary><code>client.vin.<a href="src/osintcat/vin/client.py">query</a>(...) -> VinResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Decodes vehicle identification numbers and answers catalogue questions (makes, models, manufacturers, vehicle variables). Choose what to do with `type`.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, the lookup can continue at a per-lookup price charged to your balance (the module's page in the dashboard shows the price); a lookup that finds nothing is not charged.

Errors:
- 400 `query parameter is required`: `query` missing where the chosen type needs it.
- 400 `Maximum 50 VINs allowed`: `batch` with more than 50 VINs.
- 400 `Invalid query type / Invalid search_type`: Unknown `type` or `search_type`.

Docs: https://docs.osintcat.net/api-reference/endpoint/vin
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.vin.query(
    query="1HGCM82633A004352",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**operation:** `typing.Optional[QueryVinRequestType]` — `decode` (default), `batch`, `wmi`, `makes`, `manufacturers`, `variables` or `canadian`.
    
</dd>
</dl>

<dl>
<dd>

**query:** `typing.Optional[str]` — The VIN (`decode`), VINs separated by new lines, up to 50 (`batch`), a WMI (`wmi`), or the make / manufacturer / variable name the chosen `search_type` needs. Not needed for `makes`+`all`, `manufacturers`+`all`/`parts`, `variables`+`list` and `canadian`.
    
</dd>
</dl>

<dl>
<dd>

**model_year:** `typing.Optional[float]` — `decode`: model year, improves accuracy.
    
</dd>
</dl>

<dl>
<dd>

**extended:** `typing.Optional[bool]` — `decode`: `true` for the extended field set.
    
</dd>
</dl>

<dl>
<dd>

**search_type:** `typing.Optional[str]` — `makes`: `all`, `manufacturer`, `vehicletype`, `models`, `vehicletypes`. `manufacturers`: `all`, `details`, `wmis`, `parts`. `variables`: `list`, `values`.
    
</dd>
</dl>

<dl>
<dd>

**year:** `typing.Optional[float]` — `makes` with `manufacturer`/`models`, and `canadian`: model year.
    
</dd>
</dl>

<dl>
<dd>

**vehicle_type:** `typing.Optional[str]` — `makes`+`models` and `manufacturers`+`wmis`: vehicle type filter.
    
</dd>
</dl>

<dl>
<dd>

**mfr_type:** `typing.Optional[str]` — `manufacturers`+`all`: manufacturer type filter.
    
</dd>
</dl>

<dl>
<dd>

**page:** `typing.Optional[float]` — `manufacturers`+`all`/`parts`: page number.
    
</dd>
</dl>

<dl>
<dd>

**parts_type:** `typing.Optional[str]` — `manufacturers`+`parts`: CFR part, default `565`.
    
</dd>
</dl>

<dl>
<dd>

**from_date:** `typing.Optional[str]` — `manufacturers`+`parts`: start date (required there).
    
</dd>
</dl>

<dl>
<dd>

**to_date:** `typing.Optional[str]` — `manufacturers`+`parts`: end date (required there).
    
</dd>
</dl>

<dl>
<dd>

**make:** `typing.Optional[str]` — `canadian`: make.
    
</dd>
</dl>

<dl>
<dd>

**model:** `typing.Optional[str]` — `canadian`: model.
    
</dd>
</dl>

<dl>
<dd>

**units:** `typing.Optional[QueryVinRequestUnits]` — `canadian`: `Metric` (default) or `US`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Chile
<details><summary><code>client.chile.<a href="src/osintcat/chile/client.py">person</a>(...) -> ChileanNameResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Searches Chilean public records for people by name.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, requests are refused with `429 LIMIT_REACHED` until it resets at 00:00 UTC.

Docs: https://docs.osintcat.net/api-reference/endpoint/chilean-name
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.chile.person(
    query="Juan Perez",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — A full or partial name.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.chile.<a href="src/osintcat/chile/client.py">vehicle</a>(...) -> ChileanCarResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Searches Chilean vehicle records by licence plate.

Counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, requests are refused with `429 LIMIT_REACHED` until it resets at 00:00 UTC.

Docs: https://docs.osintcat.net/api-reference/endpoint/chilean-car
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.chile.vehicle(
    query="ABCD12",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — The licence plate.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## MachineViewer
<details><summary><code>client.machine_viewer.<a href="src/osintcat/machine_viewer/client.py">stats</a>() -> MachineViewerStats</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

How many machines, files, passwords, tokens, cookies and payment cards the Machine Viewer holds.

Every Machine Viewer request counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, requests can continue at a per-lookup price charged to your balance; a search that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/machine-viewer
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.machine_viewer.stats()

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.machine_viewer.<a href="src/osintcat/machine_viewer/client.py">search</a>(...) -> MachineSearchResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Searches machines by name, username, hostname or machine ID.

Every Machine Viewer request counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, requests can continue at a per-lookup price charged to your balance; a search that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/machine-viewer
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.machine_viewer.search(
    query="DESKTOP",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**query:** `str` — What to search for: a name, username, hostname or machine ID.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.machine_viewer.<a href="src/osintcat/machine_viewer/client.py">machine</a>(...) -> MachineInfoResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

One machine, with the e-mail addresses and tokens found on it.

Every Machine Viewer request counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, requests can continue at a per-lookup price charged to your balance; a search that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/machine-viewer
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.machine_viewer.machine(
    machine_id="742e1f66-f449-4a6c-80d5-8f5eb8e9c2b5",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**machine_id:** `str` — Machine ID (a UUID), from `machineViewer.search`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.machine_viewer.<a href="src/osintcat/machine_viewer/client.py">files</a>(...) -> MachineFilesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Every file of a machine, with its path and size. Use a file's `id` with `machineViewer.file`.

Every Machine Viewer request counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, requests can continue at a per-lookup price charged to your balance; a search that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/machine-viewer
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.machine_viewer.files(
    machine_id="742e1f66-f449-4a6c-80d5-8f5eb8e9c2b5",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**machine_id:** `str` — Machine ID (a UUID), from `machineViewer.search`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.machine_viewer.<a href="src/osintcat/machine_viewer/client.py">file</a>(...) -> MachineFileResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

One file, with its content as text.

Every Machine Viewer request counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, requests can continue at a per-lookup price charged to your balance; a search that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/machine-viewer
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.machine_viewer.file(
    file_id="aB3dE5fG7hI9jK1lM3nO",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**file_id:** `str` — File ID, from `machineViewer.files`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.machine_viewer.<a href="src/osintcat/machine_viewer/client.py">download_file</a>(...) -> typing.Iterator[bytes]</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

The file itself, as it was in the log.

Every Machine Viewer request counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, requests can continue at a per-lookup price charged to your balance; a search that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/machine-viewer
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.machine_viewer.download_file(
    file_id="file_id",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**file_id:** `str` — File ID, from `machineViewer.files`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.machine_viewer.<a href="src/osintcat/machine_viewer/client.py">download_machine</a>(...) -> typing.Iterator[bytes]</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Every file of a machine in one ZIP archive.

Every Machine Viewer request counts as one lookup against your plan's daily allowance (Max: unlimited). When the allowance is used up, requests can continue at a per-lookup price charged to your balance; a search that finds nothing is not charged.

Docs: https://docs.osintcat.net/api-reference/endpoint/machine-viewer
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```python
from osintcat import OsintCat
from osintcat.environment import OsintCatEnvironment

client = OsintCat(
    api_key="<value>",
    environment=OsintCatEnvironment.PRODUCTION,
)

client.machine_viewer.download_machine(
    machine_id="machine_id",
)

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**machine_id:** `str` — Machine ID (a UUID), from `machineViewer.search`.
    
</dd>
</dl>

<dl>
<dd>

**request_options:** `typing.Optional[RequestOptions]` — Request-specific configuration.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

