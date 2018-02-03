# ajjayhierarchy
LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Company.csv" AS csvLine 
MERGE (company:Company {code: csvLine.code});

LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Brand.csv" AS csvLine 
MATCH (parent:Company {code: csvLine.parent_code})
MERGE (brand:Brand {code: csvLine.code})
MERGE (brand)-[:PART_OF { code: "part_of" }]->(parent);


LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Department.csv" AS csvLine 
MATCH (parent:Brand {code: csvLine.parent_code})
MERGE (current:Department {code:csvLine.code,name:csvLine.name,description:csvLine.description})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);


LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-SubDepartment.csv" AS csvLine 
MATCH (parent:Department {code: csvLine.parent_code})
MERGE (current:SubDepartment {code:csvLine.code,name:csvLine.name,description:csvLine.description})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);


LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Category.csv" AS csvLine 
MATCH (parent:SubDepartment {code: csvLine.parent_code})
MERGE (current:Category {code:csvLine.code,name:csvLine.name,description:csvLine.description})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);


LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-SubCategory.csv" AS csvLine 
MATCH (parent:Category {code: csvLine.parent_code})
MERGE (current:SubCategory {code:csvLine.code,name:csvLine.name,description:csvLine.description})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);



LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Products.csv" AS csvLine 
MATCH (parent:SubCategory {code: csvLine.parent_code})
MERGE (current:Product {productid:csvLine.productid,name:csvLine.name,sku:csvLine.sku,
price:toFloat(csvLine.price),qty:toInt(csvLine.qty),description:csvLine.description,activity:csvLine.activity,
gender:csvLine.gender,
special_price:toFloat(csvLine.special_price)
})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);