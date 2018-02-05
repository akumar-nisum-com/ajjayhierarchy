# ajjayhierarchy
LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Company.csv" AS csvLine 
MERGE (company:Company:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description})

LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Brand.csv" AS csvLine 
MATCH (parent:Company {code: csvLine.parent_code})
MERGE (brand:Brand:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description})
MERGE (brand)-[:PART_OF { code: "part_of" }]->(parent);


LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Department.csv" AS csvLine 
MATCH (parent:Brand {code: csvLine.parent_code})
MERGE (current:Department:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);


LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-SubDepartment.csv" AS csvLine 
MATCH (parent:Department {code: csvLine.parent_code})
MERGE (current:SubDepartment:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);


LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Category.csv" AS csvLine 
MATCH (parent:SubDepartment {code: csvLine.parent_code})
MERGE (current:Category:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);


LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-SubCategory.csv" AS csvLine 
MATCH (parent:Category {code: csvLine.parent_code})
MERGE (current:SubCategory:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);



LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Products.csv" AS csvLine 
MATCH (parent:SubCategory {code: csvLine.parent_code})
MERGE (current:Product:NX_Taxonomy {productid:csvLine.productid,name:csvLine.name,sku:csvLine.sku,
price:toFloat(csvLine.price),qty:toInteger(csvLine.qty),description:csvLine.description,activity:csvLine.activity,
gender:csvLine.gender,
special_price:toFloat(csvLine.special_price)
})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);




LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Promotions.csv" AS csvLine 
MERGE (current:Offer {code:csvLine.code,name:csvLine.name,description:csvLine.description,offer_type:csvLine.offer_type,
discount_type:csvLine.discount_type,start_date:csvLine.start_date,end_date:csvLine.end_date,active:toInteger(csvLine.active),
discount_value:toFloat(csvLine.discount_value),min_qty:toInteger(csvLine.min_qty),priority:toInteger(csvLine.priority),exclusive:toInteger(csvLine.exclusive)});


MATCH (o:Offer{code:'OO1'}),(p:Product{productid:'WJ05-XS-Brown'}) MERGE (o)-[:AVAILABLE]->(p) ;
MATCH (o:Offer{code:'OO2'}),(p:Product{productid:'WJ05-XS-Brown'}) MERGE (o)-[:AVAILABLE]->(p) ;
MATCH (o:Offer{code:'OO3'}),(p:SubCategory{code:'yoga'}) MERGE (o)-[:AVAILABLE]->(p) ;
MATCH (o:Offer{code:'OO4'}),(p:Category{code:'jackets'}) MERGE (o)-[:AVAILABLE]->(p) ;
MATCH (o:Offer{code:'OO5'}),(p:Department{code:'men'}) MERGE (o)-[:AVAILABLE]->(p) ;
