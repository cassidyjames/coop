# Submit Items

Send content to Coop for automated rule evaluation. Every time you submit an item, Coop runs it through all your configured [Automated Rules](../user/rules.md).

Submit items when they are created, edited, reported, or otherwise need to be re-evaluated. You should also submit items retroactively if you configure new rules after launch.

## Endpoint

```
POST /api/v1/items/async/
```

Authentication: `X-API-KEY` header. See [API Keys and Authentication](../development/api-auth.md).

## Request

```json
{
  "items": [
    {
      "id": "unique-item-id-123",
      "typeId": "your-item-type-id",
      "data": {
        "fieldName1": "value1",
        "fieldName2": 123
      }
    }
  ]
}
```

You can submit multiple items in a single request by including additional objects in the `items` array. All processing is asynchronous.

### Request body fields

| Field                       | Type   | Required? | Description                                                              |
| :-------------------------- | :----- | :-------- | :----------------------------------------------------------------------- |
| `items`                     | Array  | Required  | One or more items to submit                                              |
| `items[].id`                | String | Required  | Your unique identifier for this item                                     |
| `items[].typeId`            | String | Required  | The Coop Item Type ID for this item, as configured in the dashboard      |
| `items[].data`              | Object | Required  | The item payload. Fields must match the schema defined for the Item Type |
| `items[].typeVersion`       | String | Optional  | Version string for schema versioning                                     |
| `items[].typeSchemaVariant` | String | Optional  | Schema variant identifier                                                |

### Formatting `data` fields

The shape of `data` must match your Item Type's schema. Common field types:

| Field type            | Format                                              |
| :-------------------- | :-------------------------------------------------- |
| String                | Plain string value                                  |
| Number                | JSON number                                         |
| Boolean               | `true` or `false`                                   |
| Image / Audio / Video | URL string pointing to the media                    |
| Geohash               | Base-32 geohash string                              |
| Datetime              | ISO 8601 string (e.g. `"2024-01-15T10:30:00.000Z"`) |
| Related Item          | `{ "id": "...", "typeId": "..." }` object           |

## Response

| Status            | Meaning                                       |
| :---------------- | :-------------------------------------------- |
| `202 Accepted`    | Items received and queued for rule evaluation |
| `400 Bad Request` | Validation failure — see [Errors](errors.md)  |
| `401 / 403`       | Authentication failure                        |

See [Errors](errors.md) for the full error response format.

## Notes

- Processing is asynchronous by default. If you need synchronous processing (results in the response), contact support.
- If a rule matches and triggers an action, Coop sends a POST request to your [action callback endpoint](actions.md).
- For background on Item Types and how items are identified in Coop, see [Basic Concepts](../user/concepts.md).
