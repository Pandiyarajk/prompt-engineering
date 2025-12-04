# Module 5: Prompt Engineering for Developers

## Lesson 3: Full-Stack Development

---

## 🎯 Lesson Overview

**Time Required:** 75 minutes  
**Difficulty:** Advanced  
**Prerequisites:** Lessons 1 and 2 completed

In this lesson, you'll learn:
- How to generate RESTful APIs
- Creating optimized database queries
- Building frontend components
- Implementing algorithms
- Performance optimization techniques

---

## 🔌 API Development

### Generate Complete REST API

**Prompt:**

```
Act as a Backend Developer expert in RESTful API design.

Create a complete REST API for a Product management system.

Endpoints:
- POST /api/products - Create product
- GET /api/products - List products (pagination, filtering, sorting)
- GET /api/products/{id} - Get single product
- PUT /api/products/{id} - Update product
- PATCH /api/products/{id} - Partial update
- DELETE /api/products/{id} - Delete product
- GET /api/products/search - Search products
- GET /api/products/categories - Get categories

Product Fields:
- id, name, description, price, category, sku
- stock_quantity, is_active, images, tags
- created_at, updated_at

Requirements:
- Input validation with Pydantic
- Authentication (JWT Bearer)
- Role-based authorization (admin for create/update/delete)
- Error handling with proper HTTP status codes
- Rate limiting (100 requests/minute)
- Request logging
- OpenAPI documentation
- Pagination with cursor-based and offset options
- Filtering by category, price range, availability
- Sorting by price, name, date
- Full-text search

Technical Stack:
- Python 3.11+
- FastAPI
- SQLAlchemy 2.0 (async)
- Pydantic v2

Include:
- All endpoint implementations
- Request/response schemas
- Database models
- Authentication middleware
- Error handlers
- Example curl requests
- OpenAPI schema customization
```

**Example API Endpoint:**

```python
"""
Product API endpoints.
"""
from typing import Optional, List
from fastapi import APIRouter, Depends, HTTPException, Query, status
from sqlalchemy.ext.asyncio import AsyncSession

from app.database import get_db
from app.auth import get_current_user, require_admin
from app.models.user import User
from app.schemas.product import (
    ProductCreate, ProductUpdate, ProductResponse,
    ProductListResponse, ProductSearchParams
)
from app.services.product_service import ProductService

router = APIRouter(prefix="/api/products", tags=["Products"])


@router.get(
    "",
    response_model=ProductListResponse,
    summary="List Products",
    description="Get paginated list of products with optional filtering and sorting."
)
async def list_products(
    page: int = Query(1, ge=1, description="Page number"),
    limit: int = Query(20, ge=1, le=100, description="Items per page"),
    category: Optional[str] = Query(None, description="Filter by category"),
    min_price: Optional[float] = Query(None, ge=0, description="Minimum price"),
    max_price: Optional[float] = Query(None, ge=0, description="Maximum price"),
    is_active: Optional[bool] = Query(None, description="Filter by active status"),
    sort_by: str = Query("created_at", description="Sort field"),
    sort_order: str = Query("desc", regex="^(asc|desc)$"),
    db: AsyncSession = Depends(get_db),
    current_user: User = Depends(get_current_user)
) -> ProductListResponse:
    """
    List products with pagination and filtering.
    
    - **page**: Page number (1-indexed)
    - **limit**: Number of items per page (max 100)
    - **category**: Filter by category name
    - **min_price/max_price**: Filter by price range
    - **is_active**: Filter by active status
    - **sort_by**: Field to sort by (name, price, created_at)
    - **sort_order**: Sort direction (asc, desc)
    """
    service = ProductService(db)
    
    filters = ProductSearchParams(
        category=category,
        min_price=min_price,
        max_price=max_price,
        is_active=is_active
    )
    
    products, total = await service.list_products(
        page=page,
        limit=limit,
        filters=filters,
        sort_by=sort_by,
        sort_order=sort_order
    )
    
    return ProductListResponse(
        items=products,
        total=total,
        page=page,
        limit=limit,
        has_next=page * limit < total
    )


@router.post(
    "",
    response_model=ProductResponse,
    status_code=status.HTTP_201_CREATED,
    summary="Create Product",
    description="Create a new product. Requires admin role."
)
async def create_product(
    product_data: ProductCreate,
    db: AsyncSession = Depends(get_db),
    current_user: User = Depends(require_admin)
) -> ProductResponse:
    """
    Create a new product.
    
    Requires admin authentication.
    """
    service = ProductService(db)
    product = await service.create_product(product_data)
    return product


@router.get(
    "/{product_id}",
    response_model=ProductResponse,
    summary="Get Product",
    description="Get a single product by ID."
)
async def get_product(
    product_id: str,
    db: AsyncSession = Depends(get_db)
) -> ProductResponse:
    """Get product by ID."""
    service = ProductService(db)
    product = await service.get_product(product_id)
    
    if not product:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"Product {product_id} not found"
        )
    
    return product


@router.put(
    "/{product_id}",
    response_model=ProductResponse,
    summary="Update Product",
    description="Update all fields of a product. Requires admin role."
)
async def update_product(
    product_id: str,
    product_data: ProductCreate,
    db: AsyncSession = Depends(get_db),
    current_user: User = Depends(require_admin)
) -> ProductResponse:
    """Update product (full update)."""
    service = ProductService(db)
    product = await service.update_product(product_id, product_data)
    
    if not product:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"Product {product_id} not found"
        )
    
    return product


@router.delete(
    "/{product_id}",
    status_code=status.HTTP_204_NO_CONTENT,
    summary="Delete Product",
    description="Delete a product. Requires admin role."
)
async def delete_product(
    product_id: str,
    db: AsyncSession = Depends(get_db),
    current_user: User = Depends(require_admin)
) -> None:
    """Delete product by ID."""
    service = ProductService(db)
    success = await service.delete_product(product_id)
    
    if not success:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"Product {product_id} not found"
        )


@router.get(
    "/search",
    response_model=ProductListResponse,
    summary="Search Products",
    description="Full-text search across products."
)
async def search_products(
    q: str = Query(..., min_length=2, description="Search query"),
    page: int = Query(1, ge=1),
    limit: int = Query(20, ge=1, le=100),
    db: AsyncSession = Depends(get_db)
) -> ProductListResponse:
    """
    Search products by name, description, or tags.
    
    - **q**: Search query (minimum 2 characters)
    """
    service = ProductService(db)
    products, total = await service.search_products(q, page, limit)
    
    return ProductListResponse(
        items=products,
        total=total,
        page=page,
        limit=limit,
        has_next=page * limit < total
    )
```

---

## 🗄️ Database Queries

### Generate Optimized Queries

**Prompt:**

```
Act as a Database Expert specializing in PostgreSQL and SQLAlchemy.

Create optimized database queries for an e-commerce analytics dashboard.

Queries Needed:
1. Top 10 best-selling products (last 30 days)
2. Revenue by category (this month vs last month with % change)
3. Customer lifetime value (top 100 customers)
4. Cart abandonment rate (last 7 days)
5. Average order value trend (daily for last 30 days)
6. Inventory alerts (low stock products)

Database Schema:
- orders (id, user_id, total, status, created_at, completed_at)
- order_items (id, order_id, product_id, quantity, unit_price)
- products (id, name, category_id, price, stock_quantity)
- categories (id, name)
- users (id, email, created_at)
- carts (id, user_id, created_at, updated_at, status)
- cart_items (id, cart_id, product_id, quantity)

For each query provide:
1. Raw optimized SQL
2. SQLAlchemy ORM equivalent (async)
3. Expected execution plan notes
4. Index recommendations
5. Explanation of optimization

Use CTEs, window functions, and proper indexing strategies.
```

**Example Query Output:**

```python
"""
E-commerce Analytics Queries.
"""
from datetime import datetime, timedelta
from typing import List, Tuple
from sqlalchemy import select, func, and_, case, text
from sqlalchemy.ext.asyncio import AsyncSession
from dataclasses import dataclass


@dataclass
class TopProduct:
    """Top selling product result."""
    product_id: str
    product_name: str
    category_name: str
    total_quantity: int
    total_revenue: float
    order_count: int


async def get_top_selling_products(
    db: AsyncSession,
    days: int = 30,
    limit: int = 10
) -> List[TopProduct]:
    """
    Get top selling products by quantity sold.
    
    SQL Query:
    ```sql
    SELECT 
        p.id,
        p.name,
        c.name as category_name,
        SUM(oi.quantity) as total_quantity,
        SUM(oi.quantity * oi.unit_price) as total_revenue,
        COUNT(DISTINCT o.id) as order_count
    FROM products p
    JOIN order_items oi ON p.id = oi.product_id
    JOIN orders o ON oi.order_id = o.id
    LEFT JOIN categories c ON p.category_id = c.id
    WHERE o.status = 'completed'
      AND o.completed_at >= NOW() - INTERVAL '30 days'
    GROUP BY p.id, p.name, c.name
    ORDER BY total_quantity DESC
    LIMIT 10;
    ```
    
    Index Recommendations:
    - CREATE INDEX idx_orders_completed_status ON orders(completed_at, status);
    - CREATE INDEX idx_order_items_product ON order_items(product_id);
    """
    cutoff_date = datetime.utcnow() - timedelta(days=days)
    
    query = (
        select(
            Product.id,
            Product.name,
            Category.name.label('category_name'),
            func.sum(OrderItem.quantity).label('total_quantity'),
            func.sum(OrderItem.quantity * OrderItem.unit_price).label('total_revenue'),
            func.count(func.distinct(Order.id)).label('order_count')
        )
        .join(OrderItem, Product.id == OrderItem.product_id)
        .join(Order, OrderItem.order_id == Order.id)
        .outerjoin(Category, Product.category_id == Category.id)
        .where(
            and_(
                Order.status == 'completed',
                Order.completed_at >= cutoff_date
            )
        )
        .group_by(Product.id, Product.name, Category.name)
        .order_by(func.sum(OrderItem.quantity).desc())
        .limit(limit)
    )
    
    result = await db.execute(query)
    rows = result.all()
    
    return [
        TopProduct(
            product_id=str(row.id),
            product_name=row.name,
            category_name=row.category_name or "Uncategorized",
            total_quantity=row.total_quantity,
            total_revenue=float(row.total_revenue),
            order_count=row.order_count
        )
        for row in rows
    ]


@dataclass
class CategoryRevenue:
    """Revenue by category comparison."""
    category_name: str
    current_month_revenue: float
    previous_month_revenue: float
    change_percentage: float


async def get_revenue_by_category_comparison(
    db: AsyncSession
) -> List[CategoryRevenue]:
    """
    Compare revenue by category between current and previous month.
    
    SQL Query:
    ```sql
    WITH monthly_revenue AS (
        SELECT 
            c.name as category_name,
            SUM(CASE 
                WHEN DATE_TRUNC('month', o.completed_at) = DATE_TRUNC('month', CURRENT_DATE)
                THEN oi.quantity * oi.unit_price 
                ELSE 0 
            END) as current_month,
            SUM(CASE 
                WHEN DATE_TRUNC('month', o.completed_at) = DATE_TRUNC('month', CURRENT_DATE - INTERVAL '1 month')
                THEN oi.quantity * oi.unit_price 
                ELSE 0 
            END) as previous_month
        FROM categories c
        LEFT JOIN products p ON c.id = p.category_id
        LEFT JOIN order_items oi ON p.id = oi.product_id
        LEFT JOIN orders o ON oi.order_id = o.id AND o.status = 'completed'
        GROUP BY c.name
    )
    SELECT 
        category_name,
        current_month,
        previous_month,
        CASE 
            WHEN previous_month > 0 
            THEN ((current_month - previous_month) / previous_month * 100)
            ELSE 0 
        END as change_percentage
    FROM monthly_revenue
    ORDER BY current_month DESC;
    ```
    """
    now = datetime.utcnow()
    current_month_start = now.replace(day=1, hour=0, minute=0, second=0, microsecond=0)
    previous_month_start = (current_month_start - timedelta(days=1)).replace(day=1)
    
    query = text("""
        WITH monthly_revenue AS (
            SELECT 
                c.name as category_name,
                COALESCE(SUM(CASE 
                    WHEN o.completed_at >= :current_month_start
                    THEN oi.quantity * oi.unit_price 
                    ELSE 0 
                END), 0) as current_month,
                COALESCE(SUM(CASE 
                    WHEN o.completed_at >= :previous_month_start 
                      AND o.completed_at < :current_month_start
                    THEN oi.quantity * oi.unit_price 
                    ELSE 0 
                END), 0) as previous_month
            FROM categories c
            LEFT JOIN products p ON c.id = p.category_id
            LEFT JOIN order_items oi ON p.id = oi.product_id
            LEFT JOIN orders o ON oi.order_id = o.id AND o.status = 'completed'
            GROUP BY c.name
        )
        SELECT 
            category_name,
            current_month,
            previous_month,
            CASE 
                WHEN previous_month > 0 
                THEN ROUND(((current_month - previous_month) / previous_month * 100)::numeric, 2)
                ELSE 0 
            END as change_percentage
        FROM monthly_revenue
        ORDER BY current_month DESC
    """)
    
    result = await db.execute(query, {
        'current_month_start': current_month_start,
        'previous_month_start': previous_month_start
    })
    
    return [
        CategoryRevenue(
            category_name=row.category_name,
            current_month_revenue=float(row.current_month),
            previous_month_revenue=float(row.previous_month),
            change_percentage=float(row.change_percentage)
        )
        for row in result.all()
    ]


async def get_cart_abandonment_rate(db: AsyncSession, days: int = 7) -> dict:
    """
    Calculate cart abandonment rate.
    
    Abandonment = Carts with items that weren't converted to orders.
    
    SQL Query:
    ```sql
    SELECT
        COUNT(DISTINCT CASE WHEN c.status = 'abandoned' THEN c.id END) as abandoned_carts,
        COUNT(DISTINCT CASE WHEN c.status = 'converted' THEN c.id END) as converted_carts,
        COUNT(DISTINCT c.id) as total_carts,
        ROUND(
            COUNT(DISTINCT CASE WHEN c.status = 'abandoned' THEN c.id END)::numeric /
            NULLIF(COUNT(DISTINCT c.id), 0) * 100,
            2
        ) as abandonment_rate
    FROM carts c
    JOIN cart_items ci ON c.id = ci.cart_id
    WHERE c.created_at >= NOW() - INTERVAL '7 days';
    ```
    """
    cutoff_date = datetime.utcnow() - timedelta(days=days)
    
    query = text("""
        SELECT
            COUNT(DISTINCT CASE WHEN c.status = 'abandoned' THEN c.id END) as abandoned_carts,
            COUNT(DISTINCT CASE WHEN c.status = 'converted' THEN c.id END) as converted_carts,
            COUNT(DISTINCT c.id) as total_carts
        FROM carts c
        JOIN cart_items ci ON c.id = ci.cart_id
        WHERE c.created_at >= :cutoff_date
    """)
    
    result = await db.execute(query, {'cutoff_date': cutoff_date})
    row = result.one()
    
    abandonment_rate = 0.0
    if row.total_carts > 0:
        abandonment_rate = round(row.abandoned_carts / row.total_carts * 100, 2)
    
    return {
        'abandoned_carts': row.abandoned_carts,
        'converted_carts': row.converted_carts,
        'total_carts': row.total_carts,
        'abandonment_rate': abandonment_rate,
        'period_days': days
    }
```

---

## ⚛️ Frontend Components

### Generate React Component

**Prompt:**

```
Act as a Senior React Developer.

Create a reusable DataTable component for displaying tabular data.

Requirements:
- Sortable columns (click header to sort)
- Client-side pagination
- Search/filter functionality
- Selectable rows (checkbox)
- Custom cell rendering
- Loading and empty states
- Responsive design

Props:
- columns: Column definitions (key, header, sortable, render)
- data: Array of data objects
- onRowSelect: Callback when rows selected
- onSort: Callback when sort changes
- pageSize: Items per page (default 10)
- searchable: Enable search
- loading: Loading state

Technical Stack:
- React 18
- TypeScript
- Tailwind CSS
- No external table libraries

Include:
- DataTable.tsx component
- Types/interfaces
- Custom hooks (useSorting, usePagination)
- Usage example
- Unit tests
```

**Example Component:**

```tsx
// DataTable.tsx
import React, { useState, useMemo, useCallback } from 'react';

// ==================== TYPES ====================

interface Column<T> {
  key: keyof T | string;
  header: string;
  sortable?: boolean;
  width?: string;
  render?: (value: any, row: T, index: number) => React.ReactNode;
}

interface SortConfig {
  key: string;
  direction: 'asc' | 'desc';
}

interface DataTableProps<T extends Record<string, any>> {
  columns: Column<T>[];
  data: T[];
  keyField?: keyof T;
  pageSize?: number;
  searchable?: boolean;
  selectable?: boolean;
  loading?: boolean;
  emptyMessage?: string;
  onRowSelect?: (selectedRows: T[]) => void;
  onSort?: (sortConfig: SortConfig | null) => void;
  onRowClick?: (row: T) => void;
}

// ==================== HOOKS ====================

function useSorting<T>(data: T[], initialSort?: SortConfig) {
  const [sortConfig, setSortConfig] = useState<SortConfig | null>(initialSort || null);

  const sortedData = useMemo(() => {
    if (!sortConfig) return data;

    return [...data].sort((a, b) => {
      const aValue = a[sortConfig.key as keyof T];
      const bValue = b[sortConfig.key as keyof T];

      if (aValue === bValue) return 0;
      
      const comparison = aValue < bValue ? -1 : 1;
      return sortConfig.direction === 'asc' ? comparison : -comparison;
    });
  }, [data, sortConfig]);

  const requestSort = useCallback((key: string) => {
    setSortConfig(current => {
      if (current?.key === key) {
        if (current.direction === 'asc') {
          return { key, direction: 'desc' };
        }
        return null; // Reset sort
      }
      return { key, direction: 'asc' };
    });
  }, []);

  return { sortedData, sortConfig, requestSort };
}

function usePagination<T>(data: T[], pageSize: number) {
  const [currentPage, setCurrentPage] = useState(1);

  const totalPages = Math.ceil(data.length / pageSize);
  
  const paginatedData = useMemo(() => {
    const start = (currentPage - 1) * pageSize;
    return data.slice(start, start + pageSize);
  }, [data, currentPage, pageSize]);

  const goToPage = useCallback((page: number) => {
    setCurrentPage(Math.max(1, Math.min(page, totalPages)));
  }, [totalPages]);

  // Reset to page 1 when data changes
  React.useEffect(() => {
    setCurrentPage(1);
  }, [data.length]);

  return {
    paginatedData,
    currentPage,
    totalPages,
    goToPage,
    hasNext: currentPage < totalPages,
    hasPrev: currentPage > 1
  };
}

// ==================== COMPONENT ====================

function DataTable<T extends Record<string, any>>({
  columns,
  data,
  keyField = 'id' as keyof T,
  pageSize = 10,
  searchable = false,
  selectable = false,
  loading = false,
  emptyMessage = 'No data available',
  onRowSelect,
  onSort,
  onRowClick
}: DataTableProps<T>) {
  const [searchQuery, setSearchQuery] = useState('');
  const [selectedKeys, setSelectedKeys] = useState<Set<any>>(new Set());

  // Filter data by search query
  const filteredData = useMemo(() => {
    if (!searchQuery) return data;
    
    const query = searchQuery.toLowerCase();
    return data.filter(row => 
      Object.values(row).some(value => 
        String(value).toLowerCase().includes(query)
      )
    );
  }, [data, searchQuery]);

  // Sorting
  const { sortedData, sortConfig, requestSort } = useSorting(filteredData);

  // Pagination
  const {
    paginatedData,
    currentPage,
    totalPages,
    goToPage,
    hasNext,
    hasPrev
  } = usePagination(sortedData, pageSize);

  // Handle sort with callback
  const handleSort = useCallback((key: string) => {
    requestSort(key);
    onSort?.(sortConfig);
  }, [requestSort, onSort, sortConfig]);

  // Handle row selection
  const handleSelectRow = useCallback((row: T) => {
    const key = row[keyField];
    setSelectedKeys(prev => {
      const next = new Set(prev);
      if (next.has(key)) {
        next.delete(key);
      } else {
        next.add(key);
      }
      
      const selectedRows = data.filter(r => next.has(r[keyField]));
      onRowSelect?.(selectedRows);
      
      return next;
    });
  }, [data, keyField, onRowSelect]);

  // Handle select all
  const handleSelectAll = useCallback(() => {
    if (selectedKeys.size === paginatedData.length) {
      setSelectedKeys(new Set());
      onRowSelect?.([]);
    } else {
      const allKeys = new Set(paginatedData.map(row => row[keyField]));
      setSelectedKeys(allKeys);
      onRowSelect?.(paginatedData);
    }
  }, [paginatedData, keyField, selectedKeys.size, onRowSelect]);

  // Get cell value
  const getCellValue = (row: T, column: Column<T>, index: number) => {
    const value = row[column.key as keyof T];
    
    if (column.render) {
      return column.render(value, row, index);
    }
    
    return value?.toString() ?? '-';
  };

  // Sort indicator
  const getSortIndicator = (key: string) => {
    if (sortConfig?.key !== key) return '↕️';
    return sortConfig.direction === 'asc' ? '↑' : '↓';
  };

  return (
    <div className="w-full">
      {/* Search Bar */}
      {searchable && (
        <div className="mb-4">
          <input
            type="text"
            placeholder="Search..."
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
            className="w-full md:w-64 px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
          />
        </div>
      )}

      {/* Table */}
      <div className="overflow-x-auto border rounded-lg">
        <table className="w-full table-auto">
          <thead className="bg-gray-50">
            <tr>
              {selectable && (
                <th className="px-4 py-3 text-left">
                  <input
                    type="checkbox"
                    checked={selectedKeys.size === paginatedData.length && paginatedData.length > 0}
                    onChange={handleSelectAll}
                    className="rounded"
                  />
                </th>
              )}
              {columns.map((column) => (
                <th
                  key={String(column.key)}
                  className={`px-4 py-3 text-left text-sm font-semibold text-gray-700 ${
                    column.sortable ? 'cursor-pointer hover:bg-gray-100' : ''
                  }`}
                  style={{ width: column.width }}
                  onClick={() => column.sortable && handleSort(String(column.key))}
                >
                  <div className="flex items-center gap-2">
                    {column.header}
                    {column.sortable && (
                      <span className="text-gray-400">
                        {getSortIndicator(String(column.key))}
                      </span>
                    )}
                  </div>
                </th>
              ))}
            </tr>
          </thead>
          
          <tbody className="divide-y">
            {loading ? (
              <tr>
                <td colSpan={columns.length + (selectable ? 1 : 0)} className="px-4 py-8 text-center">
                  <div className="flex items-center justify-center gap-2">
                    <div className="animate-spin h-5 w-5 border-2 border-blue-500 border-t-transparent rounded-full" />
                    Loading...
                  </div>
                </td>
              </tr>
            ) : paginatedData.length === 0 ? (
              <tr>
                <td colSpan={columns.length + (selectable ? 1 : 0)} className="px-4 py-8 text-center text-gray-500">
                  {emptyMessage}
                </td>
              </tr>
            ) : (
              paginatedData.map((row, index) => (
                <tr
                  key={String(row[keyField])}
                  className={`hover:bg-gray-50 ${onRowClick ? 'cursor-pointer' : ''} ${
                    selectedKeys.has(row[keyField]) ? 'bg-blue-50' : ''
                  }`}
                  onClick={() => onRowClick?.(row)}
                >
                  {selectable && (
                    <td className="px-4 py-3" onClick={(e) => e.stopPropagation()}>
                      <input
                        type="checkbox"
                        checked={selectedKeys.has(row[keyField])}
                        onChange={() => handleSelectRow(row)}
                        className="rounded"
                      />
                    </td>
                  )}
                  {columns.map((column) => (
                    <td key={String(column.key)} className="px-4 py-3 text-sm">
                      {getCellValue(row, column, index)}
                    </td>
                  ))}
                </tr>
              ))
            )}
          </tbody>
        </table>
      </div>

      {/* Pagination */}
      {totalPages > 1 && (
        <div className="flex items-center justify-between mt-4 px-2">
          <span className="text-sm text-gray-600">
            Showing {(currentPage - 1) * pageSize + 1} to{' '}
            {Math.min(currentPage * pageSize, sortedData.length)} of {sortedData.length}
          </span>
          
          <div className="flex gap-2">
            <button
              onClick={() => goToPage(currentPage - 1)}
              disabled={!hasPrev}
              className="px-3 py-1 border rounded disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-50"
            >
              Previous
            </button>
            
            <span className="px-3 py-1">
              Page {currentPage} of {totalPages}
            </span>
            
            <button
              onClick={() => goToPage(currentPage + 1)}
              disabled={!hasNext}
              className="px-3 py-1 border rounded disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-50"
            >
              Next
            </button>
          </div>
        </div>
      )}
    </div>
  );
}

export default DataTable;
```

---

## 🔢 Algorithm Implementation

### Generate Optimized Algorithm

**Prompt:**

```
Act as an Algorithms Expert.

Implement efficient search and sort algorithms in Python.

Algorithms Needed:
1. Binary Search (iterative and recursive)
2. Quick Sort (with optimizations)
3. Merge Sort
4. Graph BFS and DFS
5. Dijkstra's shortest path

For each algorithm:
- Efficient implementation
- Handle edge cases
- Clear variable names
- Step-by-step comments
- Time complexity: O(?)
- Space complexity: O(?)
- When to use vs alternatives

Include:
- Implementation
- Complexity analysis in docstring
- Unit tests with various inputs
- Performance benchmarks
- Practical usage examples
```

---

## 💻 Practical Exercises

### Exercise 5.5: Generate API Endpoint

**Task:** Create a complete API for a blog post system with:
- CRUD operations
- Comments support
- Tags/categories
- Full-text search
- Rate limiting

**Write your prompt:**
```
[Your prompt here]
```

---

### Exercise 5.6: Create React Form Component

**Task:** Create a dynamic form component with:
- Field configuration via props
- Validation (required, patterns, custom)
- Multiple field types (text, email, select, checkbox)
- Error display
- Form submission handling

**Write your prompt:**
```
[Your prompt here]
```

---

## 📝 Key Takeaways

```
┌─────────────────────────────────────────────────────────────────┐
│  LESSON 3 KEY TAKEAWAYS                                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✅ AI generates complete API implementations                   │
│                                                                  │
│  ✅ Request index recommendations with queries                  │
│                                                                  │
│  ✅ Generate React components with hooks and TypeScript         │
│                                                                  │
│  ✅ Always include complexity analysis for algorithms           │
│                                                                  │
│  ✅ Test all generated code thoroughly                          │
│                                                                  │
│  ✅ Customize generated code for your specific needs            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✅ Module 5 Complete! 🎉

Congratulations! You've mastered AI-powered development!

**You can now:**
- ✅ Generate production-quality code
- ✅ Refactor complex code effectively
- ✅ Implement design patterns correctly
- ✅ Build complete REST APIs
- ✅ Create optimized database queries
- ✅ Generate React components
- ✅ Implement efficient algorithms

---

## 🔗 Next Steps

| Your Interest | Next Module |
|---------------|-------------|
| **Advanced prompting** | [Module 6: Advanced Techniques](../module-6-advanced-techniques/README.md) |
| **AI Agents** | [Module 7: AI Agents](../module-7-ai-agents/README.md) |
| **Hands-on project** | [Project 3: Code Review Bot](../../projects/project-3-code-review-bot/) |

---

**Outstanding work completing Module 5! 🚀**
