---
tags:
  - ai
  - mcp
---
# MCP

Protocol cung cấp khả năng streaming.
Giúp centralize connection đến data bằng 1 server.

Base trên TCP protocol. Cơ chế JSON-RPC

## Features

Hỗ trợ distributed STDIO -> Xử lý file trực tiếp
HTTP cho internet qua SSE. -> Tương tác với internet, trả data real time.

## Resource và tool

Resource là data read only, không được sửa
MCP tool là các command được cung cấp để ai sử dụng.

## Pagination

MCP server tạo và hold snapshot của dữ liệu. Mỗi response sẽ trả về kèm 1 pointer và 1 end pointer. Nếu chưa khớp thì sẽ lấy dữ liệu tiếp cho đến khi xong.