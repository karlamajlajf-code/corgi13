# Tejo Beauty - Code Style & Conventions

## General Principles
- **Type Safety First:** Use strict TypeScript, avoid `any` types
- **Functional Preferred:** Favor functional components (React), pure functions
- **DRY Principle:** Extract shared logic to utilities/composables
- **Explicit over Implicit:** Clear naming, explicit return types
- **Performance Conscious:** Memoization, lazy loading, code splitting

## TypeScript Standards

### Type Annotations
```typescript
// ✅ GOOD: Explicit return types
export function calculateTotal(items: CartItem[]): number {
  return items.reduce((sum, item) => sum + item.price, 0);
}

// ❌ BAD: Implicit any
export function processData(data) {
  return data.map(item => item.value);
}

// ✅ GOOD: Proper generic types
export function fetchData<T>(url: string): Promise<T> {
  return axios.get<T>(url).then(res => res.data);
}
```

### Naming Conventions
- **Interfaces/Types:** PascalCase with descriptive names
  - Prefer `type` for unions/primitives, `interface` for object shapes
  - Suffix: `Props`, `Config`, `Response`, `DTO`, `Entity`
- **Functions/Methods:** camelCase, verb-based names
- **Components:** PascalCase (React), descriptive names
- **Constants:** SCREAMING_SNAKE_CASE or camelCase for config objects
- **Files:** kebab-case for utilities, PascalCase for components

```typescript
// Types & Interfaces
interface UserProfile { /* ... */ }
type OrderStatus = 'pending' | 'completed' | 'cancelled';

// Constants
const API_BASE_URL = 'https://api.example.com';
const defaultConfig = { timeout: 5000 };

// Functions
function calculateDiscount(price: number, coupon: Coupon): number { /* ... */ }

// Components
function ProductCard({ product }: ProductCardProps) { /* ... */ }
```

## React/Next.js Conventions

### Component Structure
```typescript
// ✅ GOOD: Functional component with TypeScript
interface ProductCardProps {
  product: Product;
  onAddToCart: (productId: string) => void;
}

export function ProductCard({ product, onAddToCart }: ProductCardProps) {
  const handleClick = () => {
    onAddToCart(product.id);
  };

  return (
    <div className="product-card">
      <h3>{product.name}</h3>
      <button onClick={handleClick}>Add to Cart</button>
    </div>
  );
}
```

### Hooks Usage
- Use `use` prefix for custom hooks
- Extract complex logic into custom hooks
- Use `useCallback` for event handlers passed to children
- Use `useMemo` for expensive computations

### State Management
- **Local state:** useState for component-specific state
- **Global state:** Zustand stores (see `stores/` directory)
- **Server state:** TanStack Query for API data
- **Form state:** React Hook Form + Zod validation

## NestJS Backend Conventions

### Module Structure
```typescript
// ✅ GOOD: Proper NestJS module structure
@Module({
  imports: [TypeOrmModule.forFeature([Product])],
  controllers: [ProductController],
  providers: [ProductService],
  exports: [ProductService],
})
export class ProductModule {}
```

### DTOs & Validation
```typescript
// ✅ GOOD: Use class-validator decorators
export class CreateProductDto {
  @IsString()
  @IsNotEmpty()
  name: string;

  @IsNumber()
  @Min(0)
  price: number;

  @IsOptional()
  @IsString()
  description?: string;
}
```

### Service Layer
- Keep controllers thin, business logic in services
- Use dependency injection
- Return typed responses
- Handle errors with proper HTTP exceptions

## Formatting (Prettier)

**Configuration (.prettierrc):**
```json
{
  "singleQuote": true,
  "semi": true,
  "printWidth": 100,
  "trailingComma": "es5"
}
```

**Rules:**
- Single quotes for strings
- Semicolons required
- 100 character line width
- ES5 trailing commas

## ESLint Rules
- **No unused variables:** Remove or prefix with `_`
- **No console.log:** Use proper logging (pino for backend)
- **Explicit return types:** For exported functions
- **No floating promises:** Always await or handle promises

## Git Commit Conventions

### Format
```
<type>(<scope>): <subject>

<body (optional)>

<footer (optional)>
```

### Types
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Code refactoring (no functionality change)
- `perf`: Performance improvement
- `style`: Formatting, whitespace (no code change)
- `test`: Add/update tests
- `docs`: Documentation updates
- `chore`: Build, dependencies, tooling

### Examples
```
feat(products): add product filtering by category

fix(auth): resolve JWT token expiration issue

refactor(api): extract common validation logic to shared util

perf(images): implement lazy loading for product images
```

## Documentation

### JSDoc Comments
```typescript
/**
 * Calculates the final price after applying discount and tax
 * @param basePrice - The original product price
 * @param discount - Discount percentage (0-100)
 * @param taxRate - Tax rate as decimal (e.g., 0.08 for 8%)
 * @returns The final price including discount and tax
 * @throws {ValidationError} If discount is invalid
 */
export function calculateFinalPrice(
  basePrice: number,
  discount: number,
  taxRate: number
): number {
  // Implementation
}
```

### README Standards
- Each package/app should have README.md
- Include: Purpose, installation, usage, API reference
- Keep up-to-date with code changes

## Testing Conventions

### File Naming
- Unit tests: `<filename>.spec.ts`
- E2E tests: `<feature>.e2e-spec.ts`
- Test utilities: `test-utils.ts`

### Test Structure
```typescript
describe('ProductService', () => {
  describe('findAll', () => {
    it('should return array of products', async () => {
      // Arrange
      const mockProducts = [{ id: '1', name: 'Test' }];
      
      // Act
      const result = await service.findAll();
      
      // Assert
      expect(result).toEqual(mockProducts);
    });
  });
});
```

## Performance Guidelines
- Use dynamic imports for code splitting
- Implement proper caching strategies
- Optimize images (next/image component)
- Lazy load components below the fold
- Debounce search/filter inputs
- Use React.memo for expensive components
