```go
type VaultConfig struct {

	// Enabled enables or disables Vault support.
  // Q - Why shoud below below an address ref ?
	Enabled *bool `mapstructure:"enabled"`
}

// In test file
func makeMaya(t testing.TB) () {	
	conf := DefaultConfig()
  
  // Tip - Did you know conversion of a literal to a ref ?
  conf.Vault.Enabled = new(bool)
}
```
