# External sorting 

Lecture - https://www.youtube.com/watch?v=YjFI9CJy6x0

**Definition** - when a collection of records is too large to fit im main memory.  

Let's say **user** model has a lot of fields (name,surname,id.email...). If we want to make a sort according to ID , it's inefficient to load whole user because of IO. That is why we need to use **key sort**. 

## Key Sort
All keys are stored in separate **index file**. Each key is stored along side with the pointer that shows the position of corresponding record in the original data file. 
- They key file should be smaller than original file.
- The index file will be reordered for sorting requiring less IO.  
- If there multiple fields for sorting then multiple index files are created. 
