# M100 : MongoDB for SQL Pros

# Chapter 1 : Concepts of RDMBS and MongoDB

## MongoDB in Five Minutes

- Fault Tolerance : DB의 중복 복사본을 생성하여 단일 서버 오류에 대항
- Scalability : 서버를 추가하여 부하를 분산
- Data Near Users : 빠른 액세스를 위해 전세계의 가까운 사용자에게 데이터를 보관

## NoSQL Databases

### Initial Reason for NoSQL Databases

- Data Explosion
- Bigger Server
- NoSQL

### NoSQL Families

- Key/value
    - Simple
    - Key points to information
    - Database scan be partitioned
- Graph Database
    - Niche problems
    - Relations within table
    - SQL statements with self-joins
- Column Oriented or Wide Column
    - Data is stored per column
    - Grate for analytics
- Document Database
    - Polymorphic data structures
    - Obvious relationships using embedded arrays and documents
    - Easy and natural representation
    - No complex mapping between application data and database