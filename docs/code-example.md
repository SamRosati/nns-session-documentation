# An example of a codeblock for javascript #

## Backend Routes (index.js) ##

```js title="index.js" linenums="1" hl_lines="2-4"
import { Router } from 'express';

const router = Router();

// Health Check
router.get('/health', (req, res) => {
    res.json({ status: 'ok', timestamp: new Date().toISOString() });
});

import authRoutes from './auth.routes';
import tournamentRoutes from './tournament.routes';
import uploadRoutes from './upload.routes';

router.use('/auth', authRoutes);
router.use('/tournaments', tournamentRoutes);
router.use('/upload', uploadRoutes);

export default router;
```