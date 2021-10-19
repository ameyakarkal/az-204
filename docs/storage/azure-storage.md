# azure storage account

## Concept
- Azure Storage Account
    - blob storage has containers
    - containers contain blobs
    - blobs have unique URI
- Access
    - Shared key on entire storage account
    - SAS token

- Blob Types
    - BlockBlob : requirement to create a whole new file
    - AppendBlob : add updates at the end of the file (logging)
    - PageBlob : random access to data stored in a file (disk)

- Storage Account Kind
    -v1
        - Blob (Block | Append | Page)
        - Table
        - Queue
        - File
    - BlobStorage Account
        - Blob
    - standard v2
            - Blob (Block | Append | Page)
            - Table
            - Queue
            - File
    - premium v2
        - v2 account
            - Blob (Page)
        - BlockBlob account
            - Blob (Block | Append)
        - File account
            - File
- Replication
    - LRS : local redundent storage
        - three copies are created in ONE region under ONE zone (data center)
    - ZRS : zone redundent storage
        - three copies are created in ONE region across THREE zones (data centers)
    - GRS : geo redundent storage
        - three copies are created in ONE region under ONE zone (data center)
        - three copies are created in another region under ONE zone (data center)
        - read/write on primary
        - fail over which takes an hour needed to read from secondary
    - GZRS : geo zone redundent storage
        - three copies are created in ONE region across THREE zones (data centers)
        - three copies are created in another region under ONE zone (data centers)
        - read/write on primary
        - read/write on primary
        - fail over which takes an hour needed to read from secondary
    - RA-GRS : read access geo redundent storage
        - same as GRS with secondary read
    - RA-GZRS read access geo zone redundent storage
        - same as GZRS with secondary read
- Properties and Metadata
    - Properties
        - etag
        - lastmodified
        - content-type
    - Metadata
    - dictionary on blob

- Access Tiers
    - supported by v2 and blobstorage v1
    - on account
        - hot
        - cool
    - on blob
        - hot
        - cool
        - archieve
- Blob Life Cycle
    - move | change tier | delete blobs with last updated datetime stamp
- Soft Delete
    - on storage account.
    - specify number of days till which a blob can be marked as deleted and then recovered
    - enabled "Show Deleted Blobs"
- Snapshots and versions
    - versioning need to be enabled on the storage account
    - create version
    - make a specific version current version
    - adds version parameters in the uri with SAS token
    

- Leases
    - acquire lease, specify lease id with every request to change blob
- Immutable blobs
    - policy
        - time based : blob cannot be edited. blob can be deleted after the time duration
        - legal hold using tags : blob cannot be edited or deleted
    - defined on container
    - if blob has a tag, implies that the policy is applied

- Tools
    - azure cli
:::mermaid
    classDiagram
    direction LR

    class StorageAccount {
        String name
        String uri
        List<Container>
        Replication
        Access
    }
    class Container{
        List<Blob>
    }

    class Blob {
        String path
    }

    StorageAccount*--Container
    Container*--Blob
:::

blob types

:::mermaid
    classDiagram

    class Blob {
        +String uri

    }

    class BlockBlob{        
    }
    class AppendBlob{        
    }
    class PageBlob{        
    }

    Blob<--BlockBlob
    Blob<--AppendBlob
    Blob<--PageBlob
:::

## Storage Account Kinds

:::mermaid

    flowchart RL
    sa("StorageAccount")

    subgraph sav1 [Storage Account v1]
        direction TB
        blob
        table
        queue
        file
    end

    subgraph sablob [Block Blob Account]
        blobv1(blob)
    end

    subgraph sav2 [Storage Account v2]

    end

    subgraph standard
        blob2(blob)
        table2(table)
        queue2(queue)
        file2(file)
    end

    subgraph premium
        blob3(blob with page)
        blob4("blob with blob | append")
        file3(file)
    end

    sav1 --> sa
    sablob --> sa
    standard --> sa
    premium --> sa
    
:::

### Replication 