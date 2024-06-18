## Reporting MySQL Queries

These are handy MySQL queries for you to quey `equalify_db.sql.gz`. You can also use the [Equalify MySQL Query Builder](https://chatgpt.com/g/g-UprUAfKUa-equalify-mysql-query-builder) to help build unique queries.

## Get URLs Related to a Property
```
SELECT u.url_id, u.url
FROM urls u
JOIN properties p ON u.url_property_id = p.property_id
WHERE p.property_id = 1111
```

## Get Nodes Related to a URL
```
SELECT node_id, node_equalified, node_html, node_targets
FROM nodes
WHERE node_url_id = 16970
```

## Get Messages Related to a URL
```
SELECT DISTINCT m.message_id, m.message, m.message_type
FROM messages m
JOIN message_nodes mn ON m.message_id = mn.message_id
JOIN nodes n ON mn.node_id = n.node_id
WHERE n.node_url_id = 16970
```

## Get Message and Node Count Related to a URL
```
SELECT m.message_id, m.message, m.message_type, COUNT(n.node_id) AS related_nodes_count
FROM messages m
JOIN message_nodes mn ON m.message_id = mn.message_id
JOIN nodes n ON mn.node_id = n.node_id
WHERE n.node_url_id = 16970 AND n.node_equalified = 0
GROUP BY m.message_id, m.message, m.message_type
ORDER BY related_nodes_count DESC
```

## Get Message, Node Count, Message Type and Example URL related to a Property
```
SELECT 
    m.message_id,
    m.message,
    m.message_type,
    COUNT(n.node_id) AS node_count,
    MIN(u.url) AS example_url
FROM 
    messages m
JOIN 
    message_nodes mn ON m.message_id = mn.message_id
JOIN 
    nodes n ON mn.node_id = n.node_id
JOIN 
    urls u ON n.node_url_id = u.url_id
JOIN 
    properties p ON u.url_property_id = p.property_id
WHERE 
    p.property_id = 215
    AND n.node_equalified = 0
GROUP BY 
    m.message_id, m.message, m.message_type
```
