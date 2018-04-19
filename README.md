# ajjayhierarchy
LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Company.csv" AS csvLine 
MERGE (company:Company:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description,node_type:1,created_on:timestamp(),updated_on:timestamp()})

CREATE CONSTRAINT ON (t:Company) ASSERT t.code IS UNIQUE;

LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Brand.csv" AS csvLine 
MATCH (parent:Company {code: csvLine.parent_code})
MERGE (brand:Brand:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description,node_type:2,role:'root',created_on:timestamp(),updated_on:timestamp()})
MERGE (brand)-[:PART_OF { code: "part_of" }]->(parent);


CREATE CONSTRAINT ON (t:Brand) ASSERT t.code IS UNIQUE;

LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Department.csv" AS csvLine 
MATCH (parent:Brand {code: csvLine.parent_code})
MERGE (current:Department:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description,node_type:3,created_on:timestamp(),updated_on:timestamp()})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);

CREATE CONSTRAINT ON (t:Department) ASSERT t.code IS UNIQUE;

LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-SubDepartment.csv" AS csvLine 
MATCH (parent:Department {code: csvLine.parent_code})
MERGE (current:SubDepartment:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description,node_type:4,created_on:timestamp()})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);

CREATE CONSTRAINT ON (t:SubDepartment) ASSERT t.code IS UNIQUE;

LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Category.csv" AS csvLine 
MATCH (parent:SubDepartment {code: csvLine.parent_code})
MERGE (current:Category:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description,node_type:5,created_on:timestamp(),updated_on:timestamp()})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);

CREATE CONSTRAINT ON (t:Category) ASSERT t.code IS UNIQUE;

LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-SubCategory.csv" AS csvLine 
MATCH (parent:Category {code: csvLine.parent_code})
MERGE (current:SubCategory:NX_Taxonomy {code:csvLine.code,name:csvLine.name,description:csvLine.description,node_type:6,created_on:timestamp(),updated_on:timestamp()})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);

CREATE CONSTRAINT ON (t:SubCategory) ASSERT t.code IS UNIQUE;

LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Products.csv" AS csvLine 
MATCH (parent:SubCategory {code: csvLine.parent_code})
MERGE (current:Product:NX_Taxonomy {productid:toUpper(csvLine.productid),name:csvLine.name,sku:toUpper(csvLine.sku),
price:toFloat(csvLine.price),qty:toInteger(csvLine.qty),description:csvLine.description,activity:csvLine.activity,
gender:csvLine.gender,
special_price:toFloat(csvLine.special_price),node_type:7,created_on:timestamp(),updated_on:timestamp()
})
MERGE (current)-[:PART_OF { code: "part_of" }]->(parent);

CREATE CONSTRAINT ON (t:Product) ASSERT t.productid IS UNIQUE;




LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/akumar-nisum-com/ajjayhierarchy/master/Trendzy-Promotions.csv" AS csvLine 
MERGE (current:Offer {code:csvLine.code,name:csvLine.name,description:csvLine.description,offer_type:csvLine.offer_type,
discount_type:csvLine.discount_type,start_date:csvLine.start_date,end_date:csvLine.end_date,active:toInteger(csvLine.active),
discount_value:toFloat(csvLine.discount_value),min_qty:toInteger(csvLine.min_qty),priority:toInteger(csvLine.priority),exclusive:toInteger(csvLine.exclusive),is_combinable:toBoolean(csvLine.is_combinable),created_on:timestamp(),updated_on:timestamp()});


CREATE CONSTRAINT ON (t:Offer) ASSERT t.code IS UNIQUE;


MATCH (o:Offer{code:'OO1'}),(p:Product{productid:'WJ05-XS-BROWN'}) MERGE (o)-[:AVAILABLE]->(p) ;

MATCH (o:Offer{code:'OO2'}),(p:Product{productid:'WJ05-XS-BROWN'}) MERGE (o)-[:AVAILABLE]->(p) ;

MATCH (o:Offer{code:'OO3'}),(p:SubCategory{code:'yoga'}) MERGE (o)-[:AVAILABLE]->(p) ;

MATCH (o:Offer{code:'OO4'}),(p:Category{code:'jackets'}) MERGE (o)-[:AVAILABLE]->(p) ;

MATCH (o:Offer{code:'OO5'}),(p:Department{code:'men'}) MERGE (o)-[:AVAILABLE]->(p) ;


MATCH (o:Offer{code:'OO6'}),(p:Product{productid:'1884502'}) MERGE (o)-[:AVAILABLE]->(p) ;

MATCH (o:Offer{code:'OO7'}),(p:Product{productid:'1884502'}) MERGE (o)-[:AVAILABLE]->(p) ;

MATCH (o:Offer{code:'OO8'}),(p:SubCategory{code:'blenders'}) MERGE (o)-[:AVAILABLE]->(p) ;

MATCH (o:Offer{code:'OO9'}),(p:Category{code:'kitchen'}) MERGE (o)-[:AVAILABLE]->(p) ;

MATCH (o:Offer{code:'OO10'}),(p:Department{code:'furniture'}) MERGE (o)-[:AVAILABLE]->(p) ;
MATCH (o:Offer{code:'OO11'}),(p:Department{code:'appliances'}) MERGE (o)-[:AVAILABLE]->(p) ;

