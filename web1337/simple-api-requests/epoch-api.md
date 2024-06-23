# Epoch API

## Get current epoch on threads

#### Verification Thread

```javascript
await web1337.getCurrentEpochOnThread("vt")
```

#### Approvement Thread

```javascript
await web1337.getCurrentEpochOnThread("at")
```

## Get current leaders on shards

```javascript
await web1337.getCurrentLeadersOnShards()
```

## Get epoch data by epoch index

<pre class="language-javascript"><code class="lang-javascript"><strong>let epochIndex = 196
</strong><strong>
</strong><strong>await web1337.getEpochDataByEpochIndex(epochIndex)
</strong></code></pre>
