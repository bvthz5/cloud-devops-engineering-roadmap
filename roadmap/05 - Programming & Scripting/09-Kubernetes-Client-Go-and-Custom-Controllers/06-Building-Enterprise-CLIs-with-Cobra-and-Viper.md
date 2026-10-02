# 06 - Building Enterprise CLIs with Cobra and Viper

## 1. Cobra Architecture: The Backbone of Kubernetes Tools

Every major Kubernetes CLI tool—including `kubectl`, `helm`, `etcdctl`, and `hugo`—is built using the **Cobra** library. Cobra provides:
- Hierarchical subcommands (`kubectl get pods`).
- POSIX-compliant flags (`-f`, `--filename`).
- Shell autocompletion generation (Bash, Zsh, Fish, PowerShell).

```text
                 [ root (ktool) ]
                        │
         ┌──────────────┴──────────────┐
         ▼                             ▼
   [ version ]                     [ cluster ]
                                       │
                        ┌──────────────┴──────────────┐
                        ▼                             ▼
                    [ inspect ]                    [ drain ]
```

---

## 2. Implementing a Production Cobra CLI

```go
package main

import (
	"fmt"
	"os"

	"github.com/spf13/cobra"
	"github.com/spf13/viper"
)

var (
	cfgFile   string
	namespace string
)

var rootCmd = &cobra.Command{
	Use:   "ktool",
	Short: "ktool is a platform engineering CLI for Kubernetes operations",
}

var inspectCmd = &cobra.Command{
	Use:   "inspect [resource-name]",
	Short: "Inspect health and resource quota for a specific deployment",
	Args:  cobra.ExactArgs(1),
	RunE: func(cmd *cobra.Command, args []string) error {
		target := args[0]
		ns := viper.GetString("namespace")
		fmt.Printf("Inspecting %s in namespace %s...
", target, ns)
		// Call client-go logic here
		return nil
	},
}

func init() {
	cobra.OnInitialize(initConfig)

	rootCmd.PersistentFlags().StringVar(&cfgFile, "config", "", "config file (default is $HOME/.ktool.yaml)")
	rootCmd.PersistentFlags().StringVarP(&namespace, "namespace", "n", "default", "Kubernetes namespace")
	
	// Bind persistent flag to Viper
	viper.BindPFlag("namespace", rootCmd.PersistentFlags().Lookup("namespace"))
	
	rootCmd.AddCommand(inspectCmd)
}

func initConfig() {
	if cfgFile != "" {
		viper.SetConfigFile(cfgFile)
	} else {
		home, _ := os.UserHomeDir()
		viper.AddConfigPath(home)
		viper.SetConfigName(".ktool")
	}
	viper.AutomaticEnv()
	_ = viper.ReadInConfig()
}

func main() {
	if err := rootCmd.Execute(); err != nil {
		os.Exit(1)
	}
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Operator SDK & Kubebuilder](./05-Operator-SDK-and-Kubebuilder-Framework.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
