# Design Patterns & Architecture Guidelines

## Architectural Patterns

### Backend (NestJS)

#### Layered Architecture
```
Controller → Service → Repository/Prisma → Database
     ↓          ↓           ↓
   HTTP      Business    Data Access
   Layer      Logic       Layer
```

**Principles:**
- Controllers: Handle HTTP requests, validate input, call services
- Services: Contain business logic, orchestrate data operations
- Repositories/Prisma: Data access layer, database operations
- DTOs: Data validation and transformation at boundaries

#### Dependency Injection
```typescript
// ✅ GOOD: Use constructor injection
@Injectable()
export class ProductService {
  constructor(
    private readonly prisma: PrismaService,
    private readonly logger: LoggerService,
    private readonly cache: CacheService,
  ) {}
  
  async findAll(): Promise<Product[]> {
    return this.prisma.product.findMany();
  }
}
```

#### Module Organization
- Feature modules (ProductModule, UserModule, etc.)
- Shared module for cross-cutting concerns
- Core module for singleton services
- Config module for environment configuration

### Frontend (Next.js/React)

#### Component Patterns

**Atomic Design Principles:**
```
atoms/         # Basic building blocks (Button, Input, Icon)
molecules/     # Simple component groups (SearchBar, ProductCard)
organisms/     # Complex UI sections (Header, ProductGrid)
templates/     # Page-level layouts
pages/         # Next.js pages (App Router)
```

**Container/Presenter Pattern:**
```typescript
// Container (Smart Component) - handles logic and state
function ProductListContainer() {
  const { data, isLoading } = useQuery('products', fetchProducts);
  const [filters, setFilters] = useState({});
  
  return (
    <ProductListPresenter
      products={data}
      isLoading={isLoading}
      filters={filters}
      onFilterChange={setFilters}
    />
  );
}

// Presenter (Dumb Component) - only renders UI
interface ProductListPresenterProps {
  products: Product[];
  isLoading: boolean;
  filters: Filters;
  onFilterChange: (filters: Filters) => void;
}

function ProductListPresenter(props: ProductListPresenterProps) {
  // Pure rendering logic
}
```

#### State Management Strategy

**Local State (useState):**
- Component-specific UI state
- Form inputs, toggles, local filters

**Global State (Zustand):**
- User session, shopping cart, preferences
- State that needs to persist across page navigation

**Server State (TanStack Query):**
- API data, cached responses
- Automatic background refetching
- Optimistic updates

```typescript
// ✅ GOOD: Zustand store structure
interface CartStore {
  items: CartItem[];
  addItem: (product: Product) => void;
  removeItem: (productId: string) => void;
  clearCart: () => void;
}

export const useCartStore = create<CartStore>((set) => ({
  items: [],
  addItem: (product) =>
    set((state) => ({
      items: [...state.items, { ...product, quantity: 1 }],
    })),
  removeItem: (productId) =>
    set((state) => ({
      items: state.items.filter((item) => item.id !== productId),
    })),
  clearCart: () => set({ items: [] }),
}));
```

## Design Patterns

### Repository Pattern (Backend)
```typescript
// Abstract repository interface
interface IRepository<T> {
  findAll(): Promise<T[]>;
  findById(id: string): Promise<T | null>;
  create(data: Partial<T>): Promise<T>;
  update(id: string, data: Partial<T>): Promise<T>;
  delete(id: string): Promise<void>;
}

// Concrete implementation
class ProductRepository implements IRepository<Product> {
  constructor(private prisma: PrismaService) {}
  
  async findAll(): Promise<Product[]> {
    return this.prisma.product.findMany();
  }
  
  // ... other methods
}
```

### Factory Pattern
```typescript
// Strategy/Factory for payment processing
interface PaymentStrategy {
  process(amount: number, details: PaymentDetails): Promise<PaymentResult>;
}

class StripePayment implements PaymentStrategy {
  async process(amount: number, details: PaymentDetails) {
    // Stripe-specific logic
  }
}

class PayPalPayment implements PaymentStrategy {
  async process(amount: number, details: PaymentDetails) {
    // PayPal-specific logic
  }
}

class PaymentFactory {
  static create(type: PaymentType): PaymentStrategy {
    switch (type) {
      case 'stripe': return new StripePayment();
      case 'paypal': return new PayPalPayment();
      default: throw new Error(`Unknown payment type: ${type}`);
    }
  }
}
```

### Middleware Pattern (NestJS)
```typescript
// Logging middleware
@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    const { method, originalUrl } = req;
    const start = Date.now();
    
    res.on('finish', () => {
      const duration = Date.now() - start;
      console.log(`${method} ${originalUrl} - ${duration}ms`);
    });
    
    next();
  }
}
```

### Interceptor Pattern (NestJS)
```typescript
// Transform response data
@Injectable()
export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>> {
  intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
    return next.handle().pipe(
      map(data => ({
        data,
        statusCode: context.switchToHttp().getResponse().statusCode,
        timestamp: new Date().toISOString(),
      }))
    );
  }
}
```

### Custom Hooks Pattern (React)
```typescript
// Reusable hook for data fetching
function useProduct(productId: string) {
  return useQuery(['product', productId], () => fetchProduct(productId), {
    enabled: !!productId,
    staleTime: 5 * 60 * 1000, // 5 minutes
    retry: 3,
  });
}

// Reusable hook for form handling
function useProductForm(initialValues: Partial<Product>) {
  const form = useForm<Product>({
    defaultValues: initialValues,
    resolver: zodResolver(productSchema),
  });
  
  const onSubmit = async (data: Product) => {
    // Submit logic
  };
  
  return { form, onSubmit };
}
```

## Error Handling

### Backend Error Strategy
```typescript
// Custom exception filters
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const status = exception.getStatus();
    
    response.status(status).json({
      statusCode: status,
      message: exception.message,
      timestamp: new Date().toISOString(),
    });
  }
}

// Business logic errors
class ProductNotFoundError extends NotFoundException {
  constructor(productId: string) {
    super(`Product with ID ${productId} not found`);
  }
}
```

### Frontend Error Boundaries
```typescript
// Global error boundary (already implemented)
<ErrorBoundary fallback={<ErrorPage />}>
  <App />
</ErrorBoundary>

// Feature-specific error handling
function ProductList() {
  const { data, error, isError } = useQuery('products', fetchProducts);
  
  if (isError) {
    return <ErrorMessage error={error} />;
  }
  
  // ... render products
}
```

## Performance Patterns

### Caching Strategy
- **Frontend:** TanStack Query with staleTime
- **Backend:** Redis for frequently accessed data
- **Database:** Prisma query result caching
- **CDN:** Static assets via CDN

### Lazy Loading
```typescript
// Component lazy loading
const AdminPanel = lazy(() => import('./AdminPanel'));

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <AdminPanel />
    </Suspense>
  );
}

// Route-based code splitting (Next.js automatic)
```

### Debouncing/Throttling
```typescript
// Search input debouncing
function SearchBar() {
  const [query, setQuery] = useState('');
  const debouncedQuery = useDebounce(query, 300);
  
  const { data } = useQuery(
    ['search', debouncedQuery],
    () => searchProducts(debouncedQuery),
    { enabled: debouncedQuery.length > 2 }
  );
  
  return <input value={query} onChange={(e) => setQuery(e.target.value)} />;
}
```

## Security Patterns

### Input Validation
- **Frontend:** Zod schemas for client-side validation
- **Backend:** class-validator decorators for DTOs
- **Database:** Prisma type safety prevents SQL injection

### Authentication Flow
```
1. User submits credentials
2. Backend validates & generates JWT
3. Frontend stores token (httpOnly cookie)
4. Subsequent requests include token in Authorization header
5. Backend validates token via Passport JWT strategy
```

### Authorization Guards
```typescript
// NestJS role-based guard
@Injectable()
export class RolesGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.get<Role[]>('roles', context.getHandler());
    const { user } = context.switchToHttp().getRequest();
    return requiredRoles.some((role) => user.roles?.includes(role));
  }
}

// Usage
@Roles('admin')
@UseGuards(JwtAuthGuard, RolesGuard)
@Delete(':id')
async deleteProduct(@Param('id') id: string) {
  // Only admins can access
}
```

## Testing Patterns

### Unit Testing (Jest)
- Arrange-Act-Assert pattern
- Mock external dependencies
- Test one thing at a time

### E2E Testing (Playwright)
- Test critical user journeys
- Use Page Object Model
- Test against staging environment

---

**Key Takeaway:** Follow these patterns to maintain consistency, improve maintainability, and ensure the codebase scales effectively. When in doubt, consult existing implementations or ask for guidance.
