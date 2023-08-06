### 2 Feb 2022 : **Release 2.1**    
🏅🎖️ **_GPU based Pre-Processing Kernel_**: By shifting pre-processing of Yolo on CUDA kernel the end-to-end latency of the detection got reduce by **2 milli-seconds** on and average.    
#### Impact : Improvement in the the FPS with same model using GPU based pre-processing.   
#### Reference: [FPS and Latency Readings](https://docs.google.com/spreadsheets/d/11xc2Lf8PM9vpDY7KQQOnLw_7c9sP2d7cUS3NKqKrUWE/edit#gid=517673727)
#### Results Snippet: Channels considered are with 30 FPS with 608 x 608 as infer resolution
| GPU Card |  Latency Old | Channels Old | Improved Latency | Improved Channels | 
|--|--|--|--|--|
|3070 Ti |  7 milli-seconds | 4 or 5 | 3 milli-seconds | 11 |
| A 5000 | 4 milli-seconds | 8 | 2 milli-seconds | 16 |
| RTX 3080 | 4 milli-seconds | 8 | 2 milli-seconds | 16 |
| RTX 3080 Ti | 4 milli-seconds | 8 | 2 milli-seconds | 16 |
| RTX 3050 | 6 milli-seconds | 5 | 4 milli-seconds | 8 |
| GTX 1060 | 12 milli-seconds | 3 | 9 milli-seconds | 4 |
| GTX 1080 TI | 7 milli-seconds | 4 or 5 | 5 milli-seconds | 5 or 6 |
| A 5000 | 4 milli-seconds | 8 | 2 milli-seconds | 16 |

🏅🎖️ **_Migrating the existing code base from Tensorrt 7.2.x to 8.x_**: For executing the Model on latest GPU card and on those with server base particularly RTX series the upgradation was necessary.
#### Impact : New syntax was introduced by Nvidia and old API's were deprecated causing us to change the code. We were able to take readings on new cards
