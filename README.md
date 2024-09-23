# HW-6
const express = require('express');
const app = express();
app.use(express.json());  // To parse JSON bodies

let items = [
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
];

// GET request to retrieve items
app.get('/api/items', (req, res) => {
    res.send(items);
});

// POST request to add an item
app.post('/api/items', (req, res) => {
    const newItem = {
        id: items.length + 1,
        name: req.body.name
    };
    items.push(newItem);
    res.status(201).send(newItem);
});

// PATCH request to update an item
app.patch('/api/items/:id', (req, res) => {
    const item = items.find(i => i.id === parseInt(req.params.id));
    if (!item) return res.status(404).send('Item not found.');

    item.name = req.body.name;
    res.send(item);
});

// DELETE request to delete an item
app.delete('/api/items/:id', (req, res) => {
    const item = items.find(i => i.id === parseInt(req.params.id));
    if (!item) return res.status(404).send('Item not found.');

    const index = items.indexOf(item);
    items.splice(index, 1);
    res.send(item);
});

// Start the server
const port = process.env.PORT || 3000;
app.listen(port, () => {
    console.log(`Server running on port ${port}`);
});
